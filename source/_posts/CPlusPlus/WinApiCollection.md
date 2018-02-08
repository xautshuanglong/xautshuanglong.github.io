---
title: WinApiCollection 常用 Windows API
date: 2016-12-18 15:11:47
category: CPP
description: 实用 API，封装成工具库，以免忘记。
tags: API
toc: true
---

> 经典 Windows API 用法说明，值得收藏···
<!-- More -->

### C
#### CopyFile
	BOOL WINAPI CopyFile(
		_In_  LPCTSTR lpExistingFileName,
		_In_  LPCTSTR lpNewFileName,
		_In_  BOOL bFailIfExists
	);
	Copy an existing file to a new file.
	lpExistingFileName[in] --> The name of existing file.
	lpNewFileName[in] --> The name of new file.
	bFailIfExists[in] --> If this parameter is TRUE and the new file specified by lpNewFileName 
	already exist, the function fails. If this parameter is FALSE and the new file already exists, 
	the function overwrites the existing file andd succeeds.
#### CreateDirectory
	BOOL CreateDirectory(
		LPCTSTR lpPathName,
		LPSECURITY_ATTRIBUTES lpSecurityAttributes 
	);
	This function creates a new directory. If the underlying file system supports security on files and 
	directories, the function applies a specified security descriptor to the new directory.
	lpPathName[in] --> Long pointer to a null-terminated string that specified the path of the directory 
	to be created.
	lpSecurityAttributes[in] --> Ignored; set to NULL.

### D
#### DeleteFile
	BOOL WINAPI DeleteFile(
		_In_ LPCTSTR lpFileName
	);
	Deletes an existing file.
	lpFileName[in] --> The name of file to be deleted.

### M
#### MoveFile
	BOOL WINAPI MoveFile(
		_In_ LPCTSTR lpExistingFileName,
		_In_ LPCTSTR lpNewFileName
	);
	Moves an existing file or a directory, including its children.
	lpExistingFileName[in] --> The current name of the file or directory on the local computer.
	lpNewFileName[in] --> The new name for the file or directory. The new file must not already exist.
	A new file may be on a different file system or drive. A new directory must be on the same drive. 

### R
#### RemoveDirectory
	BOOL RemoveDirectory(
		LPCTSTR lpPathName
	);
	This function detetes an empty directory.
	lpPathName[in] --> Pointer to a null-terminated string that specifies the path of the directory 
	to be removed. The path must specify an empty directory, and the calling process must have delete 
	access to the directory.
#### ReOpenFile
	HANDLE WINAPI ReOpenFile(
		_In_  HANDLE hOriginalFile,
		_In_  DWORD dwDesiredAccess,
		_In_  DWORD dwShareMode,
		_In_  DWORD dwFlags
	);
	Reopens the specified file system object with different access rights, sharing mode, and flas.
	hOriginalFile [in] --> A handle to the object to be reopened. The object must have been created 
	by the CreateFile function.
	dwDesiredAccess [in] --> The required access to the object. For a list of values, see File Security 
	and Access Rights. You cannot request an access mode that conflicts with the sharing mode specified 
	in a previous open request whose handle is still open.
	If this parameter is zero (0), the application can query device attributes without accessing the device. 
	This is useful if an application wants to determine the size of a floppy disk drive and the formats 
	it supports without requiring a floppy in the drive.
	dwShareMode [in] --> The sharing mode of the object. You cannot request a sharing mode that 
	conflicts with the access mode specified in a previous open request whose handle is still open.
	If this parameter is zero (0) and CreateFile succeeds, the object cannot be shared and cannot be 
	opened again until the handle is closed.

