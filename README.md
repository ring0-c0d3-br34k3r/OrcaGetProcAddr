# OrcaProcAddr

List of all currently executing processes in the system u can use this Tool For Process injection well-known techniques like "Self Injection - Classic DLL Injection - PE Injection - Process Hollowing / Run PE - Thread Execution Hijacking and more...

-------------------------
| [+] Process Name   

| [+] Process PID

| [+] THREADS CNT

| [+] PARENT PROCESS ID

| [+] CLASS BASE

| [+] PRIORITY CLASS
-------------------------


![OrcaGetProcAddr](https://github.com/pjxx01/OrcaProcAddr/assets/111459558/f6deefdf-a0e1-4459-8ddf-6354631a3974)




## source code : 




#include <windows.h>
#include <tlhelp32.h>
#include <tchar.h>
#include <stdio.h>
#include <string>
#include <string.h>
#include <iostream>

using namespace std;

 BOOL GetProcessList();
 void printError(TCHAR const* msg);


int main()
{

  printf("\n ========================================================================================\n");
  printf(" ||            Hello HacktheWorld!!, WeAre OrcaRootkit$ and this Tool will             ||\n ||            help u to takes a snapshot of currently executing processes             ||");
  printf("\n ========================================================================================\n");
  cout << "" << endl;
  GetProcessList();
  string Bye = "\n\n\n[+] DONE !\n\n";
  cout << Bye;
  getchar();
  return 0;
};

  HANDLE hProcessSnap;
  HANDLE hProcess;
  DWORD  dwPriorityClass = 0;

BOOL GetProcessList()
{
  hProcessSnap = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS ,0);

    if(INVALID_HANDLE_VALUE == hProcessSnap)
    {
    printError(TEXT("CreateToolhelp32Snapshot (all process)"));
    return(FALSE);
    }

  PROCESSENTRY32 pe32;
  pe32.dwSize = sizeof(PROCESSENTRY32);

    if(!Process32First(hProcessSnap, &pe32))
    {
     printError(TEXT("Process32First"));
     CloseHandle(hProcessSnap);
     return(FALSE);
    };


do 
{
   _tprintf(TEXT("=================================================================="));
   _tprintf(TEXT("\n############# [+] PROCESS NAME : %s\n"), pe32.szExeFile);
   _tprintf(TEXT("------------------------------------------------------------------"));


    hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, pe32.th32ProcessID);    
    if(NULL == hProcess)
    {   
     printError(TEXT("OpenProcess"));
    }
     else
         {
          dwPriorityClass = GetPriorityClass(hProcess);
          if(!dwPriorityClass)
          {
            printError(TEXT("GetPriorityClass"));
            CloseHandle(hProcess);   
          };

           _tprintf(TEXT("\n[+] PROCESS ID         : %d"),pe32.th32ProcessID);
            _tprintf(TEXT("\n[+] THREADS CNT        : %d"),pe32.cntThreads);
             _tprintf(TEXT("\n[+] PARENT PROCESS ID  : %d"),pe32.th32ParentProcessID );
              _tprintf(TEXT("\n[+] CLASS BASE         : %d"),pe32.pcPriClassBase);
             
                       if(dwPriorityClass)
                         _tprintf(TEXT("\n[+] PRIORITY CLASS     : %d\n"), dwPriorityClass);
           
         };

} while(Process32Next(hProcessSnap, &pe32));

  CloseHandle(hProcessSnap);
  return(TRUE);

};
void printError(TCHAR const* msg)
{
  DWORD eNum;
  TCHAR sysMsg[256];
  TCHAR* p;

  eNum = GetLastError( );
  FormatMessage( FORMAT_MESSAGE_FROM_SYSTEM | FORMAT_MESSAGE_IGNORE_INSERTS,
         NULL, eNum, MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT), // Default language
         sysMsg, 256, NULL);

  // Trim the end of the line and terminate it with a null
  p = sysMsg;
  while( ( *p > 31 ) || ( *p == 9 ) )
    ++p;
  do { *p-- = 0; } while( ( p >= sysMsg ) &&
                          ( ( *p == '.' ) || ( *p < 33 ) ) );

  // Display the message
  _tprintf( TEXT("\n[-] WARNING: %s failed with error %d (%s)\n"), msg, eNum, sysMsg );
}





Happy Hacking - OrcaRootkit$ Team
 
