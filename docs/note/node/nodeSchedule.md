# 定时任务（node-schedule)

## 安装

````js
npm i node-schedule -S

or 

cnpm i node-schedule -S
````

## 用法

### 1. Cron风格定时器

````js
const schedule = require('node-schedule');

const  scheduleCronstyle = ()=>{
  //每分钟的第30秒定时执行一次:
    schedule.scheduleJob('30 * * * * *',()=>{
        console.log('scheduleCronstyle:' + new Date());
    }); 
}

scheduleCronstyle();

schedule.scheduleJob的回调函数中写入要执行的任务代码，一个定时器就完成了！
````

#### 规则参数 `*代表通配符`

````js
*  *  *  *  *  *
┬ ┬ ┬ ┬ ┬ ┬
│ │ │ │ │  |
│ │ │ │ │ └ day of week (0 - 7) (0 or 7 is Sun)
│ │ │ │ └───── month (1 - 12)
│ │ │ └────────── day of month (1 - 31)
│ │ └─────────────── hour (0 - 23)
│ └──────────────────── minute (0 - 59)
└───────────────────────── second (0 - 59, OPTIONAL)
````

6个占位符从左到右分别代表：秒、分、时、日、月、周几

`*`表示通配符，匹配任意，当秒是`*`时，表示任意秒数都触发，其它类推

下面可以看看以下传入参数分别代表的意思

````js
每分钟的第30秒触发： '30 * * * * *'

每小时的1分30秒触发 ：'30 1 * * * *'

每天的凌晨1点1分30秒触发 ：'30 1 1 * * *'

每月的1日1点1分30秒触发 ：'30 1 1 1 * *'

2016年的1月1日1点1分30秒触发 ：'30 1 1 1 2016 *'

每周1的1点1分30秒触发 ：'30 1 1 * * 1'
````


````js
每个参数还可以传入数值范围:

const task1 = ()=>{
  //每分钟的1-10秒都会触发，其它通配符依次类推
  schedule.scheduleJob('1-10 * * * * *', ()=>{
    console.log('scheduleCronstyle:'+ new Date());
  })
}

task1()
````

### 2、对象文本语法定时器

````js
const schedule = require('node-schedule');

function scheduleObjectLiteralSyntax(){

    //dayOfWeek
    //month
    //dayOfMonth
    //hour
    //minute
    //second
    //每周一的下午16：11分触发，其它组合可以根据代码中的注释参数名自由组合
    schedule.scheduleJob({hour: 16, minute: 11, dayOfWeek: 1}, function(){
        console.log('scheduleObjectLiteralSyntax:' + new Date());
    });
   
}

scheduleObjectLiteralSyntax();
````
### 3、递归规则定时器

````js
const schedule = require('node-schedule');

function scheduleRecurrenceRule(){
    let rule = new schedule.RecurrenceRule();
    // rule.dayOfWeek = 2;
    // rule.month = 3;
    // rule.dayOfMonth = 1;
    // rule.hour = 1;
    // rule.minute = 42;
    rule.second = 0;
    // 每分钟第60秒时就会触发，其它组合可以根据代码中的注释参数名自由组合
    schedule.scheduleJob(rule, function(){
       console.log('scheduleRecurrenceRule:' + new Date());
    });  
}
scheduleRecurrenceRule();
````
### 3、取消定时器

调用 定时器对象的cancl()方法即可

````js
const schedule = require('node-schedule');

function scheduleCancel(){

    let counter = 1;
    const j = schedule.scheduleJob('* * * * * *', function(){
        console.log('定时器触发次数：' + counter);
        counter++;
    });

    setTimeout(function() {
        console.log('定时器取消')
      // 定时器取消
        j.cancel();   
    }, 5000);
    
}

scheduleCancel();
````