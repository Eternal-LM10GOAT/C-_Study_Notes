# feof使用误区

在文件读取过程中，不能用feof函数的返回值直接用来判断文件的是否结束。而是应用于当文件读取结束的时候，判断是读取失败结束，还是遇到文件尾结束。

文本文件读取是否结束，判断返回值是否为 EOF （ fgetc ），或者 NULL （ fgets ）
例如：

```
fgetc 判断是否为 EOF .
fgets 判断返回值是否为 NULL .
```

二进制文件的读取结束判断，判断返回值是否小于实际要读的个数。
例如：

```
fread判断返回值是否小于实际要读的个数。
```

错误代码实例：

```c++
bool Load( std::string strFileName )
{
    std::map<string,string> m_Values;
    FILE* pFile = fopen(strFileName.c_str(), "r+");

    if(pFile == NULL)
    {
        return FALSE;
    }

    CHAR szBuff[256] = {0};

    do
    {
        fgets(szBuff, 256, pFile);

        if(szBuff[0] == '#')
        {
            continue;
        }

        CHAR* pChar = strchr(szBuff, '=');
        if(pChar == NULL)
        {
            continue;
        }

        std::string strName;
        strName.assign(szBuff, pChar - szBuff);
        std::string strValue = pChar + 1;
        CommonConvert::StringTrim(strName);//祛除字符前后的空白字符
        CommonConvert::StringTrim(strValue);

        m_Values.insert(std::make_pair(strName, strValue));

    }
    while(!feof(pFile));

    fclose(pFile);

    return true;
}
```

正确示例代码：

```c++
#include <iostream>
#include <string>
#include <fstream>
#include <map>
using namespace std;
bool Load(const std::string& FileStrName)
{
    std::map<string,string> m_Values;
    ifstream file(FileStrName);
    if(!file.is_open())
        return false;
    string line;
    while(getline(file,line))
    {
        if(line.empty()&&line[0]=='#')
            continue;
        size_t eqpos = line.find('=');
        if(eqpos == std::string::npos)
            continue;
        std::string strName = line.substr(0,eqpos);
        std::string strValue = line.substr(eqpos+1);
        strName.erase(strName.find_first_not_of(" \t"));
        strName.erase(strName.find_last_not_of(" \t")+1);
        strValue.erase(strValue.find_first_not_of(" \t"));
        strValue.erase(strValue.find_last_not_of(" \t\r\n")+1);

        m_Values[strName] = strValue;
    }
    file.close();
    return true;
}
```

