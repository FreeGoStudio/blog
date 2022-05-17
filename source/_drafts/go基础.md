---
title: Go基础
mathjax: true
categories: Go
tags: Go
---

# 约定
## 文件名
Go的源文件以`.go`为后缀, 文件名均已小写字母组成, 如`scanner.go`. 如果文件名由多个部分组成. 则使用下划线`_`对他们进行分隔, 如`scanner_test.go`. 文件名不包含空格或其他特殊字符.

## 标识符
必须以字符或`_`开头, 后面接0到多个字符或数字.

# 文件操作

### 文件拷贝
``` Go
func CopyFile(src, dst string) (err error) {
	sfi, err := os.Stat(src)
	if err != nil {
		return
	}
	if !sfi.Mode().IsRegular() {
		// cannot copy non-regular files (e.g., directories,
		// symlinks, devices, etc.)
		return fmt.Errorf("CopyFile: non-regular source file %s (%q)", sfi.Name(), sfi.Mode().String())
	}
	dfi, err := os.Stat(dst)
	if err != nil {
		if !os.IsNotExist(err) {
			return
		}
	} else {
		if !(dfi.Mode().IsRegular()) {
			return fmt.Errorf("CopyFile: non-regular destination file %s (%q)", dfi.Name(), dfi.Mode().String())
		}
		if os.SameFile(sfi, dfi) {
			return
		}
	}
	if err = os.Link(src, dst); err == nil {
		return
	}
	err = copyFileContents(src, dst)
	return
}

func copyFileContents(src, dst string) (err error) {
	in, err := os.Open(src)
	if err != nil {
		return
	}
	defer in.Close()
	out, err := os.Create(dst)
	if err != nil {
		return
	}
	defer func() {
		cerr := out.Close()
		if err == nil {
			err = cerr
		}
	}()
	if _, err = io.Copy(out, in); err != nil {
		return
	}
	err = out.Sync()
	return
}
```

# 文件路径

### 获取当前执行文件的完整路径
``` GO
pwd,_ :=exec.LookPath(os.Args[0])
```

### 获取绝对路径
``` GO
absPath, _ := filepath.Abs("../..")
```

### 获取路径的目录
``` GO
directory:=filepath.Dir(path)
```

### 获取路径的文件名(文件夹名)
``` GO
basename:=filepath.Base(path)
```

### 获取上上级路径
``` GO
parentPath:=filepath.Dir(filepath.Dir(absPath))
```

# 文件夹操作

### 判断文件夹是否存在
``` GO
func PathExists(path string) (bool, error) {
	_, err := os.Stat(path)
	if err == nil {
		return true, nil
	}
	if os.IsNotExist(err) {
		return false, nil
	}
	return false, err
}
```

### 文件夹创建
只能创建一个文件夹
``` GO
err := os.Mkdir(destinationPath, os.ModePerm)
```

### 文件夹递归创建
能根据路径递归创建
``` GO
err := os.MkdirAll(destinationPath, os.ModePerm)
```

