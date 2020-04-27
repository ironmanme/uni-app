**本章内容仅选择腾讯云作为服务商时支持**

如果云函数需要定时/定期执行，即定时触发，您可以使用云函数定时触发器。已配置定时触发器的云函数，会在相应时间点被自动触发，函数的返回结果不会返回给调用方。

<!-- 在需要添加触发器的云函数目录下新建文件 `config.json`，格式如下：

```js
{
  // triggers 字段是触发器数组，目前仅支持一个触发器，即数组只能填写一个，不可添加多个
  "triggers": [
    {
      // name: 触发器的名字，规则见下方说明
      "name": "myTrigger",
      // type: 触发器类型，目前仅支持 timer （即定时触发器）
      "type": "timer",
      // config: 触发器配置，在定时触发器下，config 格式为 cron 表达式，规则见下方说明
      "config": "0 0 2 1 * * *"
    }
  ]
}
``` -->

在uniCloud web控制台点击需要添加触发器的云函数详情，创建云函数触发器，格式如下：

```js
// 参数是触发器数组，目前仅支持一个触发器，即数组只能填写一个，不可添加多个
[
  {
    // name: 触发器的名字，规则见下方说明
    "name": "myTrigger",
    // type: 触发器类型，目前仅支持 timer （即定时触发器）
    "type": "timer",
    // config: 触发器配置，在定时触发器下，config 格式为 cron 表达式，规则见下方说明
    "config": "0 0 2 1 * * *"
  }
]
```

### 字段规则
- 定时触发器名称（name） ：最大支持60个字符，支持 `a-z`, `A-Z`, `0-9`, `-` 和 `_`。必须以字母开头，且一个函数下不支持同名的多个定时触发器。
- 定时触发器触发周期 （config）：指定的函数触发时间。填写自定义标准的 Cron 表达式来决定何时触发函数。有关 Cron 表达式的更多信息，请参考以下内容。

### Cron 表达式
Cron 表达式有七个必需字段，按空格分隔。其中，每个字段都有相应的取值范围：

|排序| 字段 | 值 | 通配符 |
|--| -- | -- | -- |
|第一位| 秒 | 0 - 59的整数 | , - * / |
|第二位| 分钟 | 0 - 59的整数 | , - * / |
|第三位| 小时 | 0 - 23的整数 | , - * / |
|第四位| 日 | 1 - 31的整数（需要考虑月的天数） | , - * / |
|第五位| 月 | 1 - 12的整数或 JAN、FEB、MAR、APR、MAY、JUN、JUL、AUG、SEP、OCT、NOV和DEC | , - * / |
|第六位| 星期 | 0 - 6的整数或 MON、TUE、WED、THU、FRI、SAT和SUN，其中0指星期一，1指星期二，以此类推 | , - * / |
|第七位| 年 | 1970 - 2099的整数 | , - * / |

### 通配符

| 通配符 | 含义 |
| -- | -- |
| ，（逗号） | 代表取用逗号隔开的字符的并集。例如：在“小时”字段中 1，2，3 表示1点、2点和3点 |
| - （短横线）| 包含指定范围的所有值。例如：在“日”字段中，1 - 15包含指定月份的1号到15号 |
| * （星号） | 表示所有值。在“小时”字段中，* 表示每个小时 |
| / （正斜杠） | 指定增量。在“分钟”字段中，输入1/10以指定从第一分钟开始的每隔十分钟重复。例如，第11分钟、第21分钟和第31分钟，以此类推 |


>!在 Cron 表达式中的“日”和“星期”字段同时指定值时，两者为“或”关系，即两者的条件均生效。

### 示例
下面列举一些 Cron 表达式和相关含义：
-  */5 * * * * * * 表示每5秒触发一次
- *0 0 2 1 * * * 表示在每月的1日的凌晨2点触发
-  0 15 10 * * MON-FRI * 表示在周一到周五每天上午10:15触发
- 0 0 10,14,16 * * * * 表示在每天上午10点，下午2点，下午4点触发
-  0 */30 9-17 * * * * 表示在每天上午9点到下午5点内每半小时触发
-  0 0 12 * * WED * 表示在每个星期三中午12点触发