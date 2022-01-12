---
title: JavaScript 密码强度校验
date: 2022-01-12 18:00:00
tags: [JavaScript,密码,强度校验,password,check,strong]
categories: 饭碗(技术)
---

如果一个密码满足下述所有条件，则认为这个密码是强密码：

  &emsp;由至少`8`个字符组成。</br>
  &emsp;至少包含`一个小写`字母，`一个大写`字母，和`一个数字`。</br>
  &emsp;至少包含 `_!@#$%^&*` 特殊字符。</br>
  &emsp;同一字符`不能`连续出现或者串联出行四次 (比如 "...aaaa..."、"...abcd..."、"...1234..."、"...dcba..." 是不允许的, 但是 "...aa...a..." 如果满足其他条件也可以算是强密码)。
  
密码分为 5 个等级：</br>
  &emsp;`Very Strong`</br>
  &emsp;`Strong`</br>
  &emsp;`Medium`</br>
  &emsp;`Weak`</br>
  &emsp;`Too weak`
  

#### code
```js
const LENGTH = 1 << 0; // 长度
const LOWERCASE = 1 << 1; // 小写字母
const UPPERCASE = 1 << 2; // 大写字母
const NUMBER = 1 << 3; // 数字
const SYMBOL = 1 << 4; // 符号
const SERIES = 1 << 5; // 串联/重复

const LEVEL_TOO_WEAK = 'Too weak';
const LEVEL_WEAK = 'Weak';
const LEVEL_MEDIUM = 'Medium';
const LEVEL_STRONG = 'Strong';
const LEVEL_VERY_STRONG = 'Very Strong';

const defaultOptions = {
  minLength: 8,
  allowedSymbols: '_!@#$%^&*',
  allowLx: true,
};

function check(password = '', options = defaultOptions) {
  let level = 0;
  const rules = [
    {
      regex: '[a-z]',
      flag: LOWERCASE,
    },
    {
      regex: '[A-Z]',
      flag: UPPERCASE,
    },
    {
      regex: '[0-9]',
      flag: NUMBER,
    },
    {
      regex: `[${options.allowedSymbols}]`,
      flag: SYMBOL,
    },
  ];

  if (options.minLength <= password.length) {
    level |= LENGTH;
  } else {
    return LEVEL_TOO_WEAK;
  }

  level = rules.reduce((acc, rule) => {
    // console.log(rule.regex, new RegExp(`${rule.regex}`).test(password))
    if (new RegExp(`${rule.regex}`).test(password)) {
      acc |= rule.flag;
    }
    return acc;
  }, level);

  if (options.allowLx && lxStr(password)) {
    level |= SERIES;
  }

  const count = level.toString(2).match(/1/g).length;
  // console.log(level.toString(2))
  if (count <= 1) {
    return LEVEL_TOO_WEAK;
  } else if (count <= 2) {
    return LEVEL_WEAK;
  } else if (count <= 4) {
    return LEVEL_MEDIUM;
  } else if (count < 6) {
    return LEVEL_STRONG;
  } else {
    return LEVEL_VERY_STRONG;
  }
}

function lxStr(str, len = 4) {
  const arr = Array.from(str);
  if (str < len) {
    return false;
  }
  for (let i = 0; i < arr.length - len + 1; i++) {
    const codeArr = [];
    let same = 0;
    let order = 0;
    for (let j = 0; j < len; j++) {
      codeArr.push(str.charCodeAt(i + j));
      if (j !== 0) {
        if (codeArr[j] === codeArr[j - 1]) {
          same += 1;
        }
        if (Math.abs(codeArr[j] - codeArr[j - 1]) === 1) {
          order += 1;
        }
      }
    }

    if (same === len - 1 || order === len - 1) {
      // console.log('low', len, str);
      return false;
    }
  }
  // console.log('strong', len, str);
  return true;
}

export { check };
```

#### Test
```js
console.log('qwer', check('qwer')) // Too weak
console.log('qwer2342', check('qwer2342')) // Medium
console.log('qwerA1234&', check('qwerA1234&')) // Strong
console.log('qwerA2342&', check('qwerA2342&')) // Very Strong
```
