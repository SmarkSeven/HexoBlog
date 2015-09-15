title: Log获取详细堆栈信息
date: 2015-09-15 9:10
comments: true
tags: [aar,Log，StackTrace]
categories: [aar,Log，StackTrace]

---
　　java.util.log里定义了不同级别的输出，分别在LogCat里显示不同的颜色，以便更精准的查找。 打印日志的时候，可能我们不只是需要输出错误的msg，还希望得到调用者的堆栈(比如打log的函数名是什么，类名)，通过下面的方法可以获得这些信息：

	 private static String buildLogString(String str) {
        StackTraceElement caller = new Throwable().fillInStackTrace().getStackTrace()[2];
        StringBuilder stringBuilder = new StringBuilder();
        if (TextUtils.isEmpty($.sTAG)) {
            stringBuilder.append(caller.getMethodName())
                    .append("():")
                    .append(caller.getLineNumber())
                    .append(":")
                    .append(str);
        } else {
            stringBuilder
                    .append(caller.getFileName())
                    .append(".")
                    .append(caller.getMethodName())
                    .append("():")
                    .append(caller.getLineNumber())
                    .append(":")
                    .append(str);
        }
        return stringBuilder.toString();
    }