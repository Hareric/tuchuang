# 使用applescript一键上传图片至Github图床

### 详细教程参考该博客
https://1ili.github.io/2018/04/12/upload-resource-to-github/


### applescript代码
``` applescript
on run {input, parameters}
	--设置GitHub仓库对应的本地resource目录
	set resourcePath to "/Users/Har/tuchuang/resource/"
	set baseUrl to "https://raw.githubusercontent.com/Hareric/tuchuang/master/resource/"
	
	--获取要上传文件的路径
	set uploadfile to (item 1 of input) as alias
	set uploadPath to POSIX path of (uploadfile) as string
	
	--当前时间
	set theDate to current date
	set yearString to year of theDate as string
	set monthNum to month of theDate as integer
	if monthNum < 10 then
		set monthString to ("0" & (monthNum as string))
	else
		set monthString to (monthNum as string)
	end if
	
	--获取当前时间（年-月）
	set currentDatePath to (yearString & "-" & monthString)
	--最后文件复制到的路径
	set copyedPath to (resourcePath & "/" & currentDatePath & "/")
	
	--执行终端命令
	tell application "Terminal"
		set windowA to do script "mkdir -p " & copyedPath & " && cp " & uploadPath & " $_ " & "
	" & "cd " & resourcePath & "
	" & "git add ." & "
	" & "git commit -m 'add resource' " & "
	" & "git push origin master"
		
	end tell
	
	tell application "Finder"
		--获取文件名（会有后缀名的）
		set uploadFileName to (name of uploadfile)
	end tell
	
	set sourceUrl to (baseUrl & currentDatePath & "/" & uploadFileName)
	display dialog "上传成功后获取到的链接" default answer sourceUrl buttons {"复制", "关闭"} default button 1 with title "提示" with icon note
	
	if button returned of result = "复制" then
		--复制到剪切板
		set the clipboard to (text returned of result) as string
	end if
	
end run
```
