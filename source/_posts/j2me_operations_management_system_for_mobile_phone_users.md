---
layout: post
title: 'J2ME作业，手机用户管理系统'
date: 2008-11-20
categories: [Programming, Java]
tags: [Java, J2ME]
keywords: "Java, J2ME"
description: 
comments: true
---

又是个作业。。当时搞到两天多。。有个问题一直没解决。。得YOYO大牛指点。顿悟。。

定义一个User类，保存用户字段。

``` java
import java.util.Date;
public class User {
    private String name=new String();
    private int age=0;
    private boolean sex=true;
    private Date birthday=new Date();
    private String nativePlace=new String();
    private String postcode=new String();
    private String address=new String();
    User()
    {
    }
    public void setName(String s)
    {
        name=s;
    }
    public void setAge(int x)
    {
        age=x;
    }
    public void setSex(boolean s)
    {
        sex=s;
    }
    public void setBirthday(Date d)
    {
        birthday=d;
    }
    public void setNativePlace(String s)
    {
        nativePlace=s;
    }
    public void setPostcode(String s)
    {
        postcode=s;
    }
    public void setAddress(String s)
    {
        address=s;
    }
    public String getName()
    {
        return name;
    }
    public int getAge()
    {
        return age;
    }
    public boolean getSex()
    {
        return sex;
    }
    public Date getBirthday()
    {
        return birthday;
    }
    public String getNativePlace()
    {
        return nativePlace;
    }
    public String getPostcode()
    {
        return postcode;
    }
    public String getAddress()
    {
        return address;
    }
}
```
AlertThread类，用来实现开机进度条功能（由于没学过Thread所以写的很是不完善）

``` java
import javax.microedition.lcdui.Alert;
import javax.microedition.lcdui.Gauge;
public class AlertThread extends Thread {
    Alert al;
    public AlertThread(Alert al) {
        this.al=al;
    }
    public void run()
    {
        Gauge indicator=al.getIndicator();
        for(int i=0;i<10;i++)
        {
            indicator.setValue(i);
            try
            {
                Thread.sleep(500);
            }
            catch(Exception e)
            {
                
            }
        }
    }
}
```

这个当然是主程序了，O(∩_∩)O

``` java
import java.util.Date;
import javax.microedition.lcdui.*;
import javax.microedition.midlet.MIDlet;
import javax.microedition.midlet.MIDletStateChangeException;
public class UserManage extends MIDlet implements CommandListener{
    private User user[]=new User[100];
    private int userLen=0,sel=0;
    private Display display;
    private Form userForm;
    private List userList;
    private TextField tfName,tfAge,tfNativePlace,tfPostcode,tfAddress;
    private Command saveCommand,cancelCommand,editCommand,addCommand,deleteCommand,exitCommand,enterCommand;
    private ChoiceGroup cgSex;
    private DateField dfBirthday;
    private boolean isEdit=false;
    
    public UserManage() {
        //创建窗体，列表
        userForm=new Form("用户信息");
        userList=new List("用户列表",List.IMPLICIT);
        //创建组件
        tfName=new TextField("姓名","",50,TextField.ANY);
        tfAge=new TextField("年龄","",5,TextField.NUMERIC);
        tfNativePlace=new TextField("籍贯","",20,TextField.ANY);;
        tfPostcode=new TextField("邮编","",5,TextField.NUMERIC);
        tfAddress=new TextField("地址","",100,TextField.ANY);
        String[] sexArray=new String[]{"男","女"};
        cgSex=new ChoiceGroup("性别",ChoiceGroup.POPUP,sexArray,null);
        dfBirthday=new DateField("出生年月",DateField.DATE);
        //创建Command             
        saveCommand=new Command("保存",Command.OK,1);
        cancelCommand=new Command("取消",Command.CANCEL,1);
        addCommand=new Command("添加",Command.ITEM,1);
        editCommand=new Command("编辑",Command.ITEM,2);
        deleteCommand=new Command("删除",Command.ITEM,3);
        exitCommand=new Command("退出",Command.EXIT,1);
        enterCommand=new Command("进入",Command.OK,1);
        //添加组件
        userForm.append(tfName);
        userForm.append(tfAge);
        userForm.append(cgSex);
        userForm.append(dfBirthday);
        userForm.append(tfNativePlace);
        userForm.append(tfPostcode);
        userForm.append(tfAddress);     
        //添加Command 
        userForm.addCommand(saveCommand);
        userForm.addCommand(cancelCommand);
        userForm.setCommandListener(this);      
        userList.addCommand(addCommand);
        userList.addCommand(deleteCommand);
        userList.addCommand(editCommand);
        userList.addCommand(exitCommand);
        userList.setCommandListener(this);
        //字体设置
         
    }
    protected void destroyApp(boolean arg0) throws MIDletStateChangeException {
    }
    protected void pauseApp() {
    }
    protected void startApp() throws MIDletStateChangeException {
        display=Display.getDisplay(this);
        Alert al=new Alert("");
        al.setType(AlertType.INFO);
        al.setTimeout(Alert.FOREVER);
        Gauge g=new Gauge(null,false,5,0);
        g.getPreferredHeight();
        g.getPreferredWidth();
        al.setIndicator(g);
        al.setString("用户管理系统载入中....");
        al.addCommand(enterCommand);
        al.addCommand(exitCommand);
        al.setCommandListener(this);
        AlertThread t=new AlertThread(al);
        t.start();  
        display.setCurrent(al); 
    }
    public void commandAction(Command c,Displayable d)
    {   
        if(userForm.isShown())
        {   
            if(c==saveCommand)
            {
                user[userLen] = new User();
                user[userLen].setName(tfName.getString());
                user[userLen].setAge(Integer.parseInt(tfAge.getString()));
                user[userLen].setSex(!cgSex.isSelected(0));
                user[userLen].setBirthday(dfBirthday.getDate());
                user[userLen].setNativePlace(tfNativePlace.getString());
                user[userLen].setPostcode(tfPostcode.getString());
                user[userLen].setAddress(tfAddress.getString());
                userList.append(user[userLen].getName(),null);
                userLen++;
                if(isEdit)
                {
                    delete(sel);
                }
            }
            display.setCurrent(userList);
        }
        if(userList.isShown())
        {
            sel=userList.getSelectedIndex();
            if(c==addCommand)
            {
                tfName.setString("");
                tfAge.setString("");
                tfNativePlace.setString("");
                tfPostcode.setString("");
                tfAddress.setString("");
                cgSex.setSelectedIndex(0, true);
                dfBirthday.setDate(new Date());
                isEdit=false;
                display.setCurrent(userForm);
            }
            if(c==editCommand)
            {
                tfName.setString(user[sel].getName());
                tfAge.setString(String.valueOf(user[sel].getAge()));
                tfNativePlace.setString(user[sel].getNativePlace());
                tfPostcode.setString(user[sel].getPostcode());
                tfAddress.setString(user[sel].getAddress());
                if(user[sel].getSex())
                {
                    cgSex.setSelectedIndex(0,true);
                }
                else
                {
                    cgSex.setSelectedIndex(1,true);
                }
                dfBirthday.setDate(user[sel].getBirthday());
                isEdit=true;
                display.setCurrent(userForm);
            }
            if(c==deleteCommand)
            {
                delete(sel);
            }
            
            
        }
        if(c==cancelCommand)
        {
            display.setCurrent(userList);
        }
        if(c==exitCommand)
            {
                //destroyApp(false);
                notifyDestroyed();
            }   
        if(c==enterCommand)
        {
            display.setCurrent(userForm);
        }
    }
    public void delete(int n)
    {
        
        for(int i=n;i<userLen-1;i++)
        {
            user[i]=user[i+1];
        }
        userLen--;
        userList.delete(n);
    }
}
```