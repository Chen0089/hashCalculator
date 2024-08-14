#include <windows.h>
#include <iostream>
#include <string>
#include <openssl/sha.h> // 使用 OpenSSL 库进行 SHA-256 哈希计算

LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    static HWND hButton;
    static HWND hEdit;
    
    switch (uMsg) {
    case WM_CREATE: {
        // 创建编辑框
        hEdit = CreateWindow(TEXT("EDIT"), NULL,
            WS_CHILD | WS_VISIBLE | WS_BORDER | ES_AUTOHSCROLL,
            10, 10, 200, 25, hwnd, NULL, NULL, NULL);

        // 创建按钮
        hButton = CreateWindow(TEXT("BUTTON"), TEXT("计算哈希"),
            WS_CHILD | WS_VISIBLE | BS_PUSHBUTTON,
            10, 50, 100, 30, hwnd, (HMENU)1, NULL, NULL);
        break;
    }
    case WM_COMMAND: {
        if (LOWORD(wParam) == 1) {
            // 获取编辑框中的文本
            char buffer[256];
            GetWindowText(hEdit, buffer, 256);

            // 计算 SHA-256 哈希值
            unsigned char hash[SHA256_DIGEST_LENGTH];
            SHA256(reinterpret_cast<const unsigned char*>(buffer), strlen(buffer), hash);

            // 将哈希值转换为字符串
            char hashString[65];
            for (int i = 0; i < SHA256_DIGEST_LENGTH; i++)
                sprintf(&hashString[i * 2], "%02x", hash[i]);

            // 显示哈希值
            MessageBox(hwnd, hashString, TEXT("哈希值"), MB_OK);
        }
        break;
    }
    case WM_DESTROY: {
        PostQuitMessage(0);
        break;
    }
    default:
        return DefWindowProc(hwnd, uMsg, wParam, lParam);
    }
    return 0;
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE, LPSTR, int nCmdShow) {
    const char CLASS_NAME[] = "示例窗口类";
    
    WNDCLASS wc = { };
    wc.lpfnWndProc = WindowProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = CLASS_NAME;
    
    // 注册窗口类
    RegisterClass(&wc);
    
    // 创建窗口
    HWND hwnd = CreateWindowEx(
        0, CLASS_NAME, "哈希计算器", WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
        NULL, NULL, hInstance, NULL
    );
    
    if (hwnd == NULL) {
        return 0;
    }
    
    ShowWindow(hwnd, nCmdShow);
    UpdateWindow(hwnd);

    MSG msg;
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    
    return 0;
}
