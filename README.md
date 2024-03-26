首先导入包
···
go get github.com/nuominmin/mahonia
···

```
package main
 
import (
    "bufio"
    "code.google.com/p/mahonia"
    "log"
    "os"
    "strings"
    "time"
)
 
func main() {
    //创建日志文件
    t := time.Now()
    filepath := "./log_" + strings.Replace(t.String()[:19], ":", "_", 3) + ".txt"
    file, err := os.OpenFile(filepath, os.O_CREATE, 0666)
    if err != nil {
        log.Fatal("create log file failed!")
    }
    defer file.Close()
    wFile := bufio.NewWriter(file)
    wFile.WriteString(readfile())
    wFile.Flush()
}
 
func readfile() string {
    f, err := os.Open("ex7.txt")
    if err != nil {
        return err.Error()
    }
    defer f.Close()
    buf := make([]byte, 1024)
    //文件ex7.txt的编码是gb18030
    decoder := mahonia.NewDecoder("gb18030")
    if decoder == nil {
        return "编码不存在!"
    }
    var str string = ""
    for {
        n, _ := f.Read(buf)
        if 0 == n {
            break
        }
        //解码为UTF-8
        str += decoder.ConvertString(string(buf[:n]))
    }
    return str
}

```