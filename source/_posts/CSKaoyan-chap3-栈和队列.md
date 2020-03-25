---
title: chap3-栈和队列
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:20:46
categories:
- Notebook
- CSKaoyan
tags:
- CSKaoyan
mathjax:
---

# 括号匹配问题
// 括号匹配,遇到 '\0' 结束
// 遇到花、中、圆左括号进栈，遇到花、中、圆右括号检查栈顶元素是否为相应的左括号，
// 若是，退栈；否则匹配错误。最后栈不为空也为错误
```
bool BracketsCheck(char *str){
    InitStack(S);
    int i = 0;
    while(str[i]!='\0){
        switch(str[i]){
            case '(' : Push(S,'('),break;
            case '{' : Push(S,'{'),break;
            case '[' : Push(S,'['),break;
            case ')' : Pop(S,e);
            if(e!='(')return false; break;
            case ']' : Pop(S,e);
            if(e!='[')return false; break;
            case '}' : Pop(S,e);
            if(e!='{')return false; break;
            defaule: break;
        }
        i++;
    }
    if(!IsEmpty(S)){
        print("error");
        return false;
    }else{
        print("success")
        return True;
    }
}
```