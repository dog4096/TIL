### Javascript 새 창 띄우기
#### windows.open(URL, windowName, [windowFeatures])
url을 빈 스트링("")으로 넘기면 빈 창을 여는 것도 가능.  
리턴 된 오브젝트에 직접 코딩하여 작성하는 것도 가능.   
[windowFeatures]는 창에 대한 옵션 값을 부여하는 것이 가능하다.   
https://developer.mozilla.org/ko/docs/Web/API/Window/open#Window_features
#####Return Value
성공하면 window 오브젝트를 반환   
실패하면 null을 반환   
반환된 object의 .close()를 호출하면 닫는 것도 가능.