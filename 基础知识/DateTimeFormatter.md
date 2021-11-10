# DateTimeFormatter

    //解析日期
    String dateStr= "2016年10月25日";
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日");
    LocalDate date= LocalDate.parse(dateStr, formatter);
    
    //日期转换为字符串
    LocalDateTime now = LocalDateTime.now();
    DateTimeFormatter format = DateTimeFormatter.ofPattern("yyyy年MM月dd日 hh:mm a");
    String nowStr = now .format(format);
    System.out.println(nowStr);