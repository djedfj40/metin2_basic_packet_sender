#include "pch.h"
#include <string>
#include <Windows.h>

typedef void(__fastcall* example_function_ptr)(int);

DWORD WINAPI HackThread(HMODULE hModule)
{
        
    while (true) {
       
        if (GetAsyncKeyState(VK_SHIFT) & 1) {

            unsigned char data[] = { 0x0D, 0x01, 0x03,0x00,0x01,0x04,0x00,0x01,0x00 }; // packet buffer
            int dataSize = sizeof(data); //packet buffer size
            int esi_address = 0x0789A880; // esi address

            example_function_ptr func = (example_function_ptr)0xCA5FD0; // packet send function 
            __asm {
                mov esi, esi_address;
                mov eax, esi
                add eax, 0x34 // esi+0x34 packet buffer size 
                mov esi, dataSize
                mov[eax], esi
            }
            DWORD writeThis = esi_address + 0x2c; // esi + 0x2c  packet buffer address
            unsigned char** toWrite = reinterpret_cast<unsigned char**>(writeThis);
            unsigned char* writeAddress = reinterpret_cast<unsigned char*>(*toWrite);

            for (int i = 0; i <= dataSize; i++){ // write packet buffer
            writeAddress[i] = data[i];
            
        }
           
            
        }

        if (GetAsyncKeyState(VK_CONTROL) & 1) { // unload dll
            FreeLibraryAndExitThread(hModule, 0);
        }
    }

    return 1;
}


BOOL APIENTRY DllMain( HMODULE hModule,
                       DWORD  ul_reason_for_call,
                       LPVOID lpReserved)
    
{
    
    switch (ul_reason_for_call)
    {
    case DLL_PROCESS_ATTACH:
        CloseHandle(CreateThread(nullptr, 0, (LPTHREAD_START_ROUTINE)HackThread, hModule, 0, nullptr));
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}
