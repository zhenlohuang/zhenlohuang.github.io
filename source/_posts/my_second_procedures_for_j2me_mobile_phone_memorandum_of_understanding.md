---
layout: post
title: '我的第二个J2ME程序，手机备忘录'
date: 2008-11-15
wordpress_id: 238
categories: [Programming, Java]
tags: [Java, J2ME]
keywords: "Java, J2ME"
description: 
comments: true
---

中兴实训的第一个作业，一个简单的手机备忘录。。

由于没学过JAVA，所以写得十分的ws，大牛勿鄙。。。。

作业题目：制作一个备忘录要求（用TextBox进行信息录入,长度限定为50个汉字,录入完毕后将信息保存到内存中，保存后跳转到List列表界面,设计可以对List中的信息进行删除，修改，添加操作,进行删除时要求有Alert提示框。
 
这个题目很是抽象的说。。到现在还不是很懂。。就随便写了一个。好囧

``` java
/*Memo Design by Killua 2008.11.5*/
import javax.microedition.midlet.MIDlet;
import javax.microedition.midlet.MIDletStateChangeException;
import javax.microedition.lcdui.*;
public class Memo extends MIDlet implements CommandListener{
    private Display display;
    private String[] stringArray;
    private int tbLen,numEdit;
    private TextBox tbAdd,tbView,tbEdit;
    private List list;
    private Command saveCommand=new Command("Save",Command.OK,1);
    private Command addCommand=new Command("Add",Command.ITEM,2);
    private Command editCommand=new Command("Edit",Command.ITEM,3);
    private Command deleteCommand=new Command("Delete",Command.ITEM,4);
    private Command viewCommand=new Command("View",Command.ITEM,1);
    private Command exitCommand=new Command("Exit",Command.EXIT,5);
    private Command okCommand=new Command("OK",Command.OK,1);
    private Command cancelCommand=new Command("Cancel",Command.CANCEL,1);
    
    public Memo() {
        display=Display.getDisplay(this);
        list=new List("",Choice.IMPLICIT);
        stringArray=new String[100];
        tbAdd=new TextBox("Add","",50,TextField.ANY);
        tbEdit=new TextBox("Edit","",50,TextField.ANY);
        tbView=new TextBox("View","",50,TextField.UNEDITABLE);
        tbLen=numEdit=0;
    }
    protected void destroyApp(boolean arg0) throws MIDletStateChangeException {
        // TODO Auto-generated method stub
    }
    protected void pauseApp() {
        // TODO Auto-generated method stub
    }
    protected void startApp() throws MIDletStateChangeException {
        tbAdd.addCommand(saveCommand);
        tbAdd.addCommand(cancelCommand);
        
        tbEdit.addCommand(saveCommand);
        tbEdit.addCommand(cancelCommand);
        
        tbView.addCommand(cancelCommand);
        
        list.addCommand(addCommand);
        list.addCommand(editCommand);
        list.addCommand(deleteCommand);
        list.addCommand(viewCommand);   
        list.addCommand(exitCommand);
        
        tbAdd.setCommandListener(this);
        tbEdit.setCommandListener(this);
        tbView.setCommandListener(this);
        list.setCommandListener(this);
        
        display.setCurrent(tbAdd);  
    }
    public void commandAction(Command cmd,Displayable d)
    {
        if(tbAdd.isShown())
        {
            if(cmd==saveCommand)
            {
                String stringAdd=tbAdd.getString();
                if(!stringAdd.equals(""))
                {               
                    stringArray[tbLen++]=stringAdd;
                    list.append(stringAdd,null);
                }
                display.setCurrent(list);       
            }
        }
        if(tbEdit.isShown())
        {
            if(cmd==saveCommand)
            {
                String stringEdit=tbEdit.getString();
                stringArray[numEdit]=stringEdit;
                list.set(numEdit,stringEdit, null);
                display.setCurrent(list);
            }
        }
        if(cmd==cancelCommand)
            display.setCurrent(list);       
        if(list.isShown())
        {
            if(cmd==addCommand)
            {
                tbAdd.setString("");
                display.setCurrent(tbAdd);
            }
            if(cmd==editCommand)
            {               
                numEdit=list.getSelectedIndex();
                tbEdit.setString(stringArray[numEdit]);
                display.setCurrent(tbEdit);
            }
            if(cmd==deleteCommand)
            {
                Alert al=new Alert("");
                al.setType(AlertType.WARNING);
                al.setString("you delete the record");
                al.setTimeout(2000);
                display.setCurrent(al);
                int i=list.getSelectedIndex();
                list.delete(i);         
                for(int j=i;j<tbLen-1;j++)
                    stringArray[j]=stringArray[j+1];
                tbLen--;
                display.setCurrent(list);
            }
            if(cmd==viewCommand)
            {
                int i=list.getSelectedIndex();
                tbView.setString(stringArray[i]);
                display.setCurrent(tbView);
            }
        }       
        if(cmd==exitCommand)
        {
            notifyDestroyed();
        }
    }
}
```

本来程序最开始的时候是用TextBox数组的，可是无伦怎么改也是数组非法访问，而tbArray[tbLen++]=tbAdd;这个也不知道有什么问题，一直有错。。每次赋值都把所有的给覆盖掉了，orz。最后实在没招了，用String[]了。最后还是很ws的完成了。。


1.1版本

增加了Delete时候的判断。。。。

``` java
/*Memo Design by Killua 2008.11.5*/
import javax.microedition.midlet.MIDlet;
import javax.microedition.midlet.MIDletStateChangeException;
import javax.microedition.lcdui.*;
public class Memo extends MIDlet implements CommandListener{
    private Display display;
    private String[] stringArray;
    private int tbLen,numEdit,numDelete;
    private TextBox tbAdd,tbView,tbEdit;
    private List list;
    private Alert al;
    private Command saveCommand=new Command("Save",Command.OK,1);
    private Command addCommand=new Command("Add",Command.ITEM,2);
    private Command editCommand=new Command("Edit",Command.ITEM,3);
    private Command deleteCommand=new Command("Delete",Command.ITEM,4);
    private Command viewCommand=new Command("View",Command.ITEM,1);
    private Command exitCommand=new Command("Exit",Command.EXIT,5);
    private Command okCommand=new Command("OK",Command.OK,1);
    private Command cancelCommand=new Command("Cancel",Command.CANCEL,1);
    
    public Memo() {
        display=Display.getDisplay(this);
        list=new List("Killua's Memo",Choice.IMPLICIT);
        stringArray=new String[100];
        tbAdd=new TextBox("Add","",50,TextField.ANY);
        tbEdit=new TextBox("Edit","",50,TextField.ANY);
        tbView=new TextBox("View","",50,TextField.UNEDITABLE);
        al=new Alert("Waring");
        al.setType(AlertType.WARNING);
        al.setString("you delete the record,are you sure?");
        al.setTimeout(Alert.FOREVER);
        tbLen=numEdit=0;
    }
    protected void destroyApp(boolean arg0) throws MIDletStateChangeException {
        // TODO Auto-generated method stub
    }
    protected void pauseApp() {
        // TODO Auto-generated method stub
    }
    protected void startApp() throws MIDletStateChangeException {
        tbAdd.addCommand(saveCommand);
        tbAdd.addCommand(cancelCommand);
        
        tbEdit.addCommand(saveCommand);
        tbEdit.addCommand(cancelCommand);
        
        tbView.addCommand(cancelCommand);
        
        list.addCommand(addCommand);
        list.addCommand(editCommand);
        list.addCommand(deleteCommand);
        list.addCommand(viewCommand);   
        list.addCommand(exitCommand);
        
        al.addCommand(okCommand);
        al.addCommand(cancelCommand);
        
        al.setCommandListener(this);
        tbAdd.setCommandListener(this);
        tbEdit.setCommandListener(this);
        tbView.setCommandListener(this);
        list.setCommandListener(this);
        
        display.setCurrent(tbAdd);  
    }
    public void commandAction(Command cmd,Displayable d)
    {
        if(tbAdd.isShown())
        {
            if(cmd==saveCommand)
            {
                String stringAdd=tbAdd.getString();
                if(!stringAdd.equals(""))
                {               
                    stringArray[tbLen++]=stringAdd;
                    list.append(stringAdd,null);
                }
                display.setCurrent(list);       
            }
        }
        if(tbEdit.isShown())
        {
            if(cmd==saveCommand)
            {
                String stringEdit=tbEdit.getString();
                stringArray[numEdit]=stringEdit;
                list.set(numEdit,stringEdit, null);
                display.setCurrent(list);
            }
        }
        if(al.isShown())
        {
            if(cmd==okCommand)
            {
                list.delete(numDelete);         
                for(int j=numDelete;j<tbLen-1;j++)
                    stringArray[j]=stringArray[j+1];
                tbLen--;
                display.setCurrent(list);
            }
        }
        if(cmd==cancelCommand)
            display.setCurrent(list);       
        if(list.isShown())
        {
            if(cmd==addCommand)
            {
                tbAdd.setString("");
                display.setCurrent(tbAdd);
            }
            if(cmd==editCommand)
            {               
                numEdit=list.getSelectedIndex();
                tbEdit.setString(stringArray[numEdit]);
                display.setCurrent(tbEdit);
            }
            if(cmd==deleteCommand)
            {               
                display.setCurrent(al);
                numDelete=list.getSelectedIndex();              
            }
            if(cmd==viewCommand)
            {
                int i=list.getSelectedIndex();
                tbView.setString(stringArray[i]);
                display.setCurrent(tbView);
            }
        }       
        if(cmd==exitCommand)
        {
            notifyDestroyed();
        }
    }
}
```