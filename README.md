#include<windows.h>
#include<tchar.h>

LRESULT CALLBACK WindowProc(HWND hWnd, UINT message,
	WPARAM wParam, LPARAM lParam);


//Listing OFwin1

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,
	LPSTR lpCmdLine, int nCmdShow)
{
	WNDCLASSEX WindowClass;//structure to hold our window's attributes

	static LPCTSTR szAppName{ _T("OFWin") };
	HWND hWnd;
	MSG msg;

	WindowClass.cbSize = sizeof(WNDCLASSEX);//SET structure size

											//redrew the window if the size changes
	WindowClass.style = CS_HREDRAW | CS_VREDRAW;

	//define the message handing function
	WindowClass.lpfnWndProc = WindowProc;

	WindowClass.cbWndExtra = 0;
	WindowClass.cbClsExtra = 0;

	WindowClass.hInstance = hInstance;

	//set default application icon
	WindowClass.hInstance;
	//set window cursor to be the standerd arrow
	WindowClass.hCursor = LoadCursor(nullptr, IDC_ARROW);

	//set color(gary) brush for backgroud color
	WindowClass.hbrBackground = static_cast<HBRUSH>(GetStockObject(GRAY_BRUSH));
	WindowClass.lpszMenuName = nullptr;
	WindowClass.lpszClassName = szAppName;//set class name
	WindowClass.hIconSm = nullptr;

	//register my windows class
	RegisterClassEx(&WindowClass);

	//now creat a window
	hWnd = CreateWindow(
		szAppName,
		_T("A basic Window the HARD WAY"),
		WS_OVERLAPPEDWINDOW,
		CW_USEDEFAULT,
		CW_USEDEFAULT,
		CW_USEDEFAULT,
		CW_USEDEFAULT,
		nullptr,
		nullptr,
		hInstance,
		nullptr,
		);
	ShowWindow(hWnd, nCmdShow);
	UpdateWindow(hWnd);
	//the message loop
	while (GetMessage(&msg, nullptr, 0, 0) == TRUE)//GET any message
	{
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}
	return static_cast<int>(msg.wParam);
}





//listing ofwin_2
LRESULT CALLBACK WindowProc(HWND hWnd, UINT message,
	WPARAM wParam, LPARAM lParam)
{
	switch (message)
	{
	case WM_PAINT://message is to redraw the window
		HDC hDC;
		PAINTSTRUCT PaintSt;//structure defining area to be draw
		RECT aRect;//a  working rectangle
		hDC = BeginPaint(hWnd, &PaintSt);//prepare to draw the window

										 //Get upper left and lower right of client area
		GetClientRect(hWnd, &aRect);
		SetBkMode(hDC, TRANSPARENT);
		//Now draw the test in the window client area
		DrawText(
			hDC,
			_T("But ,soft!wat light through yonder window breaks?"),
			-1,
			&aRect,
			DT_SINGLELINE |
			DT_CENTER |
			DT_VCENTER);
		EndPaint(hWnd, &PaintSt);
		return 0;

	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;



	}

	return DefWindowProc(hWnd, message, wParam, lParam);
}


