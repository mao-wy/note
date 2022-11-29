在一些实际开发中，需要运用到加减乘除计算，int类型的数据相加不会出现问题，但小数点相加会出现精度丢失的问题
加法运算
加法函数：解决思路是将所有小数化为整数进行运算，然后再转化为小数

```js
function add (arg1, arg2) {
	let r1, r2, m;
	try {
		r1 = arg1.toString().split('.')[1].length; // 获取arg1小数位的长度
	} catch (e) {
		r1 = 0;
	}
	try {
		r2 = arg2.toString().split('.')[1].length; // 获取arg2小数位的长度
	} catch (e) {
		r2 = 0;
	}
	m = Math.pow(10, Math.max(r1, r2)); // max()函数是选出小数点位最大的参数的长度，保证将两个参数都化为整数，pow()函数是将10做指数幂操作,将参数都化为整数
	return (arg1 * m + arg2 * m )/m;
}
```



减法函数
减法函数： 大概思路和加法运算差不多，不过需要考虑到避免数据相减后小数点产生多位数和计算精度损失

```js
function numSub( arg1, arg2 ) {
	let r1, r2, m;
	let precision ; // 精度
	try {
		r1 = arg1.toString().split('.')[1].length; // 获取arg1小数位的长度
	} catch (e) {
		r1 = 0;
	}
	try {
		r2 = arg2.toString().split('.')[1].length; // 获取arg2小数位的长度
	} catch (e) {
		r2 = 0;
	}
	m = Math.pow(10, Math.max(r1, r2));
	precision = (r1 > r2) ? r1 : r2;  // 获取精度，
	return ((arg1 * m - arg2 * m)/m).toFixed(precision)
}
```


Numbere.prototype.toFixed()方法是用来显示小数点后几位的一个方法，默认为0，使用的是银行家舍入规则，即四舍六入五取偶（四舍六入五留双）五后非零就进一，五后为零看奇偶，五前为偶应舍去，五前为奇要进一。使用这个方法可以避免精度损失的问题，toFixed(2)则表示保留小数点后两位


乘法运算
乘法函数

```js
function multi (arg1, arg2) {
	let r1, r2, m;
	let precision ; // 精度
	try {
		r1 = arg1.toString().split('.')[1].length; // 获取arg1小数位的长度
	} 
	try {
		r2 = arg2.toString().split('.')[1].length; // 获取arg2小数位的长度
	} 
	m = Math.pow(10, Math.max(r1+r2));
	// 将arg1转换为字符串，去除小数点，变为一个整数，乘上m,arg2也是，两者再相乘，除去m即可得到结果
	return Number(arg1.toString().replace('.', "")) * Number(arg2.toString().replace('.', ""))/m
	// return ((arg1 * m) * (arg2 * m))/m/m    这个也可以实现
}
```



除法运算
除法函数

```js
function division(arg1, arg2) {
	let t1, t2, r1, r2;
	try{
		t1 = arg1.toString().split('.')[1].length;
	} catch(e) {
		t1 = 0;
	}
	try{
		t2 = arg2.toString().split('.')[1].length;
	} catch(e) {
		t2 = 0;
	}
	r1 = Nmuber(arg1.toString().replace('.', ''));
	r2 = Nmuber(arg2.toString().replace('.', ''));
	return (r1/r2) * Math.pow(10, t2 - t1);
}
```



```js
//num1 num2传入两个值  symbol +-*/符号
function amend(num1,num2,symbol){
  var str1=num1.toString(),str2=num2.toString(),result,str1Length,str2Length
    //解决整数没有小数点方法
    try {str1Length= str1.split('.')[1].length} catch (error) {str1Length=0}
    try {str2Length= str2.split('.')[1].length} catch (error) {str2Length=0}
    var step=Math.pow(10,Math.max(str1Length,str2Length))
    // 
    console.log(step);
    switch (symbol) {
        case "+":
            result= (num1*step+num2*step)/step
            break;
        case "-":
            result= (num1*step-num2*step)/step
            break;
        case "*":
            result= ((num1*step)*(num2*step)) / step/step
            break;
        case "/":
            result= (num1*step)/(num2*step)
            break;
        default:
            break;
    }
    return result
    
}
console.log(amend(0.1,0.2,"+"));
```

