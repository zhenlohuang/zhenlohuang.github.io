---
layout: post
title: '我的第一个MFC程序...'
date: 2010-2-2
wordpress_id: 385
categories: [Programming, C++]
tags: [C++, MFC]
keywords: "C++, MFC"
description: 
comments: true
---

``` cpp
#include <windows.h>
LRESULT CALLBACK WinProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam)
{
	switch(uMsg) {
		case WM_CHAR:
			MessageBox(hwnd, "KeyBoard Press", "MessageBox", MB_OK);
			break;
		case WM_LBUTTONDOWN:
			MessageBox(hwnd, "Mouse Clicked", "MessageBox", MB_OK);
			break;
		case WM_CLOSE:
			if (MessageBox(hwnd, "是否真的结束", "MessageBox",MB_YESNO) == IDYES) {
				DestroyWindow(hwnd);
			}
			break;
		case WM_DESTROY:
			PostQuitMessage(0);
			break;
		default:
			return DefWindowProc(hwnd, uMsg, wParam, lParam);
	}
	return 0;
}
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow)
{
	WNDCLASS winclass;
	winclass.cbClsExtra = 0;
	winclass.cbWndExtra = 0;
	winclass.hbrBackground = (HBRUSH)GetStockObject(BLACK_BRUSH);
	winclass.hCursor = LoadCursor(NULL, IDC_APPSTARTING);
	winclass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
	winclass.hInstance = hInstance;
	winclass.lpfnWndProc = WinProc;
	winclass.lpszClassName = "Test";
	winclass.lpszMenuName = NULL;
	winclass.style = CS_HREDRAW | CS_VREDRAW;
	RegisterClass(&winclass);
	HWND hwnd;
	hwnd = CreateWindow("Test", "Hello MFC", WS_OVERLAPPEDWINDOW, 100, 100, 320, 240, NULL, NULL, hInstance, NULL);
	ShowWindow(hwnd, SW_SHOWNORMAL);
	MSG msg;
	while(GetMessage(&msg, hwnd, 0,0)) {
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}
	return 0;
}
```

PS：实在没有动力学这个..........
