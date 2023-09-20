# match
# 1

```java
import java.util.*;
class Solution
{
    public static String findChar (String input) {
        // 请将您的代码逻辑填写在这里
        Map<Character, Integer> charFrequency = new LinkedHashMap<>();
        for (char c : input.toCharArray()) {
            charFrequency.put(c, charFrequency.getOrDefault(c, 0) + 1);
        }

        for (Map.Entry<Character, Integer> entry : charFrequency.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey().toString();
            }
        }
        return "无";
    }
}
```

# 2
```java
import java.util.*;

class Solution {
    public static int firstMissingPositiveNum(int[] nums) {
	         //请将您的代码逻辑写在这里
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                int temp = nums[i];
                nums[i] = nums[nums[i] - 1];
                nums[temp - 1] = temp;
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }

        return n + 1;
	}
}
```

# 3
```java
import java.math.*;
class YieldRateSolution {
    public static Object solution(int a, int b, double c) {
				// 代码写在这里
        if (a <= 0 || b <= 0 || c < 0) {
            return "-";
        }

        double principal = (double) a;
        double profit = c - principal;
        double dailyRate = profit / principal / b;
        double annualizedRate = dailyRate * 365 * 100;

        BigDecimal annualizedRateDecimal = new BigDecimal(annualizedRate);
        annualizedRateDecimal = annualizedRateDecimal.setScale(2, RoundingMode.HALF_UP);

        return annualizedRateDecimal.toString() + "%";
    }
}
```

# 4
```js
function simplifiedFractions(expr) {
// 请将您的代码填写到这里
    const n = parseInt(expr);

    function gcd(a, b) {
        while (b !== 0) {
            const temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    const result = [];
    for (let denominator = 2; denominator <= n; denominator++) {
        for (let numerator = 1; numerator < denominator; numerator++) {
            if (gcd(numerator, denominator) === 1) {
                result.push(`${numerator}/${denominator}`);
            }
        }
    }
    return result.join('、');
}

```

# 5

```js
function divide(dividend, divisor) {
// 请将您的代码填写到这里
      if (dividend === 0) return 0;
    if (divisor === 1) return dividend;
    if (divisor === -1) {
        if (dividend > -(2 ** 31)) return -dividend;
        return (2 ** 31) - 1;
    }

    const sign = (dividend < 0) ^ (divisor < 0) ? -1 : 1;

    let dividendPositive = Math.abs(dividend);
    const divisorPositive = Math.abs(divisor);

    let quotient = 0;
    let temp = 1;
    let tempDivisor = divisorPositive;

    while (dividendPositive >= (tempDivisor << 1)) {
        tempDivisor <<= 1;
        temp <<= 1;
    }

    while (dividendPositive >= divisorPositive) {
        if (dividendPositive >= tempDivisor) {
            dividendPositive -= tempDivisor;
            quotient += temp;
        }

        tempDivisor >>= 1;
        temp >>= 1;
    }

    const result = sign * quotient;
    if (result > (2 ** 31) - 1) return (2 ** 31) - 1;
    if (result < -(2 ** 31)) return -(2 ** 31);
    return result;
}

```

# 6
```js
/**
	return结果
*/
function formatTime(seconds) {
  	// 请将您的实现逻辑写在这里
    if (seconds < 0 || seconds > 359999) {
        return "Invalid input";
    }

    const padZero = (num) => num.toString().padStart(2, '0');

    const hours = Math.floor(seconds / 3600);
    const minutes = Math.floor((seconds % 3600) / 60);
    const remainingSeconds = seconds % 60;

    return `${padZero(hours)}:${padZero(minutes)}:${padZero(remainingSeconds)}`;
}
```

# 7
```sql
SELECT
    current_month.user_id AS user_id,
    last_month.money AS last_income,
    current_month.money AS current_money
FROM
    records AS current_month
        JOIN
    records AS last_month
    ON current_month.user_id = last_month.user_id
WHERE
    MONTH(current_month.date) = 11 AND
    MONTH(last_month.date) = 10 AND
    current_month.money < last_month.money;
```

# 8

```sql
SELECT
    t_category_id,
    t_category,
    t_name,
    t_price,
    first_price,
    (t_price - first_price) AS first_diff_price,
    CASE
        WHEN t_price < next_price THEN next_price
        ELSE NULL
        END AS behind_price,
    CASE
        WHEN t_price < next_price THEN t_price - next_price
        ELSE NULL
        END AS behind_diff_price
FROM (
         SELECT
             t_category_id,
             t_category,
             t_name,
             t_price,
             MIN(t_price) OVER (PARTITION BY t_category_id) AS first_price,
             LEAD(t_price) OVER (PARTITION BY t_category_id ORDER BY t_price) AS next_price
         FROM
             books
     ) AS subquery
ORDER BY
    t_category_id, t_price;
```

# 9
```sql
SELECT
    name,
    sal,
    deptname,
    ROUND((sal / SUM(sal) OVER (PARTITION BY deptname)) * 100,
          CASE
              WHEN (sal / SUM(sal) OVER (PARTITION BY deptname)) * 100 - ROUND((sal / SUM(sal) OVER (PARTITION BY deptname)) * 100, 0) = 0 THEN 0
              ELSE 2
              END
        ) AS salpercent
FROM
    emp
ORDER BY
    deptname ASC;
```

# 10
```node

function findAnagramPairs(stockCodesCh) {
     const stockCodes = stockCodesCh.split(',');
    const codeMap = new Map();
    stockCodes.forEach(code => {
        const sortedCode = code.split('').sort().join('');
        if (codeMap.has(sortedCode)) {
            codeMap.get(sortedCode).push(code);
        } else {
            codeMap.set(sortedCode, [code]);
        }
    });
    const result = Array.from(codeMap.values()).filter(group => group.length > 1);
    return JSON.stringify(result);

}
```

# 11

```node

function overdraft(balance, amount) {
  
    const balanceNum = parseFloat(balance);
    const amountNum = parseFloat(amount);

    if (balanceNum < 0) {
        return "账户余额不能小于0";
    }

    if (amountNum <= 0) {
        return "取款金额必须大于0";
    }

    if (amountNum > balanceNum) {
        return "超过取款限额";
    }

    const newBalance = balanceNum - amountNum;

    return newBalance.toFixed(2);

}
```

# 12

```oracle
SELECT DISTINCT id
FROM (
         SELECT
             id,
             timeflag,
             LEAD(timeflag, 6) OVER (PARTITION BY id ORDER BY timeflag) AS lead_time
         FROM user_profile
     )
WHERE TO_CHAR(timeflag, 'MON') = 'OCT'
  AND TO_CHAR(lead_time, 'MON') = 'OCT'
  AND lead_time - timeflag <= INTERVAL '7' DAY;
```

# 13
```oracle
SELECT
    log_date AS "登录日期",
    CASE
        WHEN new_users = 0 THEN 0
        ELSE ROUND(next_day_retention / new_users, 3)
        END AS "次日留存率"
FROM (
    SELECT log_date,
       new_users,
       LEAD(next_day_retention,1,0) OVER (ORDER BY log_date) AS next_day_retention
    FROM(
         SELECT
             log_date,
             COUNT(DISTINCT CASE WHEN seq = 1 THEN user_id END) AS new_users,
             COUNT(DISTINCT CASE WHEN seq = 2 THEN user_id END) AS next_day_retention
         FROM (
                  SELECT
                      log_date,
                      user_id,
                      ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY log_date) AS seq
                  FROM user_login
              )
         GROUP BY log_date
     ))
ORDER BY log_date;
```

# 14
```python
# 客户数据字典示例
'''
customers = [
    {'name': '张三', 'balance': 1000, 'bank': '中国银行'},
    {'name': '李四', 'balance': 2000, 'bank': '中国工商银行'},
    {'name': '王五', 'balance': 3000, 'bank': '中国建设银行'}
]
'''

def get_max_balance(customers):
    balance_max = 0
    res = []
    for i in range(len(customers)):
        if customers[i]['balance']>balance_max:
            res = [customers[i]]
            balance_max=customers[i]['balance']
        elif customers[i]['balance']==balance_max:
            res.append(customers[i])
    return res
def main(args):
    customers = args
    return get_max_balance(customers)

```

# 15

```python
# 请输入你的代码
def main(args):
    account = BankAccount("Alice", 1000)   
    
    try:   
        account.deposit(args[0])    
    except NegativeDepositError as e:
        print(e)    
        
    try:    
        account.withdraw(args[1])
    except NegativeWithdrawalError as e:
        print(e)
    except InsufficientFundsError as e:
        print(e)
    print(account.get_balance())                             
                
```

# 16

```python
import sys
def minPathSum(grid):
    # 获取网格的行数和列数
    m, n = len(grid), len(grid[0])

    # 初始化 dp 数组
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = grid[0][0]

    # 初始化第一行和第一列
    for i in range(1, m):
        dp[i][0] = dp[i - 1][0] + grid[i][0]
    for j in range(1, n):
        dp[0][j] = dp[0][j - 1] + grid[0][j]
          # --------------考生编码区-------------------------- #

    # 填充 dp 数组
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]

    # 回溯求取路径
    i, j = m - 1, n - 1
    path = [grid[i][j]]
    while i > 0 or j > 0:
        if i == 0:
            j -= 1
        elif j == 0:
            i -= 1
        else:
            if dp[i - 1][j] < dp[i][j - 1]:
                i -= 1
            else:
                j -= 1
        path.append(grid[i][j])
          # --------------考生编码区-------------------------- #
    return dp[-1][-1], path[::-1]


if __name__ == '__main__':
    print(minPathSum(eval(sys.argv[1])))
```

# 17
```shell
#!/bin/bash
# 用例变量使用，例：echo "第一个参数:"$1

# 检查是否提供了正确数量的参数
if [ $# -ne 1 ]; then
  echo "用法: $0 \"句子\""
  exit 1
fi

# 从命令行参数获取句子
sentence="$1"

# 将句子分割成单词，并过滤出长度大于6的单词
filtered_words=$(echo "$sentence" | awk '{for (i=1; i<=NF; i++) if (length($i) > 6) print $i}')

# 检查是否有符合条件的单词
if [ -n "$filtered_words" ]; then
  echo "$filtered_words"
else
  echo "没有满足条件的单词"
fi
```

# 18
```shell
#!/bin/bash
# 用例变量使用，例：echo "第一个参数:"$1
if [ $1 -lt 1 ] || [ $1 -gt 9 ]; then
    echo "范围在[1-9]"
    exit
fi

n=$1

for ((i = 1, j = 1; i <= $1; )); do
    result=$((i * j))
    printf "%d*%d=%d" "$j" "$i" "$result"
    j=$((j + 1))

    if [ $j -gt $i ]; then
        printf "\n"
        i=$((i + 1))
        j=1
    else
        printf "\t"
    fi
done
```

# 19
```shell
#!/bin/bash
# ----- 编码区 ----- #
# 创建文件夹

mkdir -p folder1/folder2
mkdir -p folder3/folder4

# ----- 编码区 ----- #
# 生成文件内容并创建文件
echo "我是文件1" > folder1/file1.txt  bs=1024 count=1
echo -n "文件2" > folder1/file2.txt  bs=2048 count=1
dd if=/dev/zero of=folder1/folder2/file3.txt bs=1024 count=1
echo "我是文件4，我有两句话" > folder3/file4.txt  bs=1024 count=1
echo -n "我是文件5，我有三句话，这是第三句" > folder3/file5.txt  bs=1024 count=1
dd if=/dev/zero of=folder3/folder4/file6.txt bs=2048 count=1
# ----- 编码区 ----- #
# 查找文件并显示信息

if [ $# -eq 1 ]; then
  filename=$1
  find_result=$(find . -type f -name "$filename")
  if [ -z "$find_result" ]; then
    echo "找不到文件:$filename"
  else
    while read -r file; do
      echo "文件路径:$file,文件名:$filename"
    done <<< "$find_result"
  fi
else
  echo "请传递文件名作为参数"
fi

# ----- 编码区 ----- #
```

# 20

```oracle
WITH TransactionInfo AS (
    SELECT
        S.NAME AS "合作商家",
        C.NAME AS "信用卡客户",
        T.ACTIONDATE AS "消费发生日期",
        LEAD(T.ACTIONDATE) OVER (PARTITION BY T.CARDID ORDER BY T.ACTIONDATE) AS "上一次消费发生日期"
    FROM T
    INNER JOIN C ON T.CARDID = C.CARDID
    INNER JOIN S ON T.STEPID = S.STEPID
)
SELECT
    "合作商家",
    "信用卡客户",
    TO_CHAR("消费发生日期", 'yyyy-mm-dd hh24:mi:ss') AS "消费发生日期",
    TO_CHAR("上一次消费发生日期", 'yyyy-mm-dd hh24:mi:ss') AS "上一次消费发生日期",
    TO_NUMBER("上一次消费发生日期" - "消费发生日期") AS "间隔天数"
FROM TransactionInfo
ORDER BY "合作商家", "消费发生日期";

```
