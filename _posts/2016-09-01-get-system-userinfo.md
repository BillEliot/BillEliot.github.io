---
layout: post
title: "C++ 获取系统信息"
desc: "C++ 获取系统信息"
date: 2016-09-01
keywords: "C++,SystemInfo"
tags: [C++,SystemInfo]
categories: [Database]
---

# 注册表操作
windows 几乎将所有的信息都记录在了注册表中。  

## 打开注册表中项
**WINADVAPI LSTATUS APIENTRY RegOpenKeyEx (  
HKEY hKey,  
LPCWSTR lpSubKey,  
DWORD ulOptions,  
REGSAM samDesired,  
PHKEY phkResult);**  
* 参数一：注册表的五大类：HKEY_CLASS_ROOT;HKEY_CURRENT_USER;HKEY_LOCAL_MACHINE;
HKEY_USERS;HKEY_CURRENT_CONFIG.  
* 参数二：注册表项的相对路径(如打开HKEY_CURRENT_USER\Software\Microsoft，需传入 Software/Microsoft)
* 参数三：MSDN：must be zero
* 参数四：读写模式
* 参数五：缓冲区HKEY

``` c++
HKEY hKey;
LONG lRetn = RegOpenKeyEx(HKEY_CURRENT_USER,_T("Software/Microsoft"),0,KEY_READ,&hKey);
if(lRetn == ERROR_SUCCESS){
    //Success
}else{
    //Failed
}
/*
do something
*/
//结束后关闭注册表项
RegCloseKey(hKey);
```

## 获取项目下键值
**LONG RegQueryValueEx(  
  HKEY hKey,  
  LPCTSTR lpValueName,  
  LPDWORD lpReserved,  
  LPDWORD lpType,  
  LPBYTE lpData,  
  LPDWORD lpcbData  
);**
* 参数一：注册表项的HKEY
* 参数二：键名
* 参数三：保留 zero
* 参数四：键类型
* 参数五：缓冲区存放键值
* 参数六：实际缓冲区大小

``` c++
DWORD dwType = REG_SZ;
DWORD dwBufferSize = 256;
CString strTempValue,strValue;

LONG lRetn = RegQueryValueEx(hKey,_T("DisplayName"),0,&dwType,(BYTE*)strTempValue.GetBuffer(dwBufferSize),&dwBuffersize);
strValue.Format(_T("%s"),strTempValue);
strTempValue.ReleaseBuffer();
if(lRetn == ERROR_SUCCESS){
    //Success
}else{
    //Failed
}
```

## 枚举项目下所有子项目
**
LONG RegEnumKeyEx(  
  HKEY hKey,  
  DWORD dwIndex,  
  LPTSTR lpName,  
  LPDWORD lpcName,  
  LPDWORD lpReserved,  
  LPTSTR lpClass,  
  LPDWORD lpcClass,  
  PFILETIME lpftLastWriteTime  
);**
* 参数一：注册表项的HKEY
* 参数二：欲获取子项索引
* 参数三：子项名缓冲区
* 参数四：实际缓冲区大小
* 参数五：保留 zero
* 参数六：项的类名缓冲区
* 参数七：实际缓冲区大小
* 参数八：枚举子项上一次修改的时间

枚举HKEY_CURRENT_USER\Software\Microsoft下的子项  

``` c++
DWORD dwIndex = 0;
CString strSubName;
DWORD dwBufferSize = 256;

LONG lRetn = RegEnumKeyEx(hKey,dwIndex,strSubName.GetBuffer(dwBufferSize),&dwBufferSize,0,NULL,NULL,NULL);
while(lRetn == ERROR_SUCCESS){
    //Success
    /*
    do something
    */
    strSubName.ReleaseBuffer();
    
    dwIndex ++;
    dwBufferSize = 256;
    RegEnumKeyEx(hKey,dwIndex,strSubName.GetBuffer(dwBufferSize),&dwBufferSize,0,NULL,NULL,NULL);
}
```

# 注册表操作实例
## 获取CPU主频
windows注册表保存CPU主频的位置：HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\CentralProcessor\0 项中名为“~MHz”键中，0表示第一个CPU，1 表示第二个CPU，以此类推。  

``` c++
DWORD GetProcessorSpeed(){
    HKEY hKey;
    DWORD dwType = REG_DWORD;
    dwSize = sizeof(DWORD);
    DWORD dwCpuSpeed = 0;

    LONG lRet = RegOpenKeyEx(HKEY_LOCAL_MACHINE, _T("HARDWARE/DESCRIPTION/System/CentralProcessor/0"),0,KEY_READ,&hKey);
    if(lRet != ERROR_SUCCESS){
        return 0;
    }
    lRet = RegQueryValueEx(hKey, _T("~MHz"), NULL,&dwType, (BYTE*)&dwCpuSpeed, &dwSize);
    RegCloseKey(hKey);
    
    if(ERROR_SUCCESS != lRet){
        return 0;
    }
    return dwCpuSpeed;
}
```
