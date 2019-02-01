<br>
**지난 2019.01.15 - 2019.01.28 동안 각 TF별로 메일 서비스 구현을 했다. 
나는 우리 서비스에서 메일 쓰기 부분 구현을 맡았다. 
구현하면서 메일 쓰기 중 적용하기 가장 어려웠던(?) tui file-uploader에 대해 글을 써보고자 한다.**

## 1. 프로젝트에 uploader 적용

##### **tui file-uploader 공식 github 주소 : https://github.com/nhnent/tui.file-uploader**

![2019.02.01-1](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019.02.01-1.PNG?raw=true)

tui file-uploader github에 README.md를 보면 위의 이미지와 CDN으로 프로젝트에 file-uploader를 적용할 수 있다. 하지만 CDN을 사용하면 우리 서비스에 맞는 기능을 구현하기가 힘들어서 직접 코드를 다운 받아서 사용했다.

![2019.02.01-2](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019.02.01-2.PNG?raw=true)

위에 나와있는 파일들을 다운받아서 사용하면 된다. 나는 production branch에서 코드를 받아와 프로젝트에 적용했다.

![2019.02.01-3](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019.02.01-3.PNG?raw=true)

file-uploader html 코드 부분은 github에서 제공해주는 example08번을 수정해서 사용했다!

![2019.02.01-4](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019.02.01-4.PNG?raw=true)

프로젝트에 html, js, css 파일을 적용한 후 uploader 객체를 생성했다. uploader는 submit이 일어나면 **지정한 send url로POST 요청을 보낸다.**
또한 한번에 여러 개의 파일을 업로드하고자 한다면 **isMultiple과 isBatchTransfer** 옵션을 true로 줘야한다. 이외에도 폴더 업로드를 위한 isFolder 등 여러 옵션들이 존재한다.

![2019.02.01-5](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019.02.01-5.PNG?raw=true)

github의 Wiki 페이지를 이용하면 더 자세한 내용들을 볼 수 있다. 나는 Wiki 창을 눌렀는데 아무것도 안 떠서 처음에 내용이 없는 줄 알았다. (github에 익숙하지 않아서...)

![2019.02.01-6](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019.02.01-6.PNG?raw=true)

이제 html 파일을 열어보면 이와 같이 적용된 tui file-uploader를 볼 수 있다!!!
기본적으로 제공하는 파일 추가, 삭제, 선택, 드랍존 등 기능을 이용할 수 있다.

다만 나에게 필요한 기능은 메일의 내용과 파일을 같이 전송하는 것이었기에 커스터마이징이 필요했다.

다음 글에서 formData와 file을 같이 전송하는 법에 대해 적도록 하겠다.
<br>

