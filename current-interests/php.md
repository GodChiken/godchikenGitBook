# PHP 세팅 이슈

* PHP 세팅 진행 중 xdebug 관련 문제 해결진행중
  * pecl 이 설치가 되어있지않아 설치 진행
  * 그후 xdebug 설치를 진행하려했으나 잘안됨
    * [https://jasonmccreary.me/articles/install-pear-pecl-mac-os-x//](https://jasonmccreary.me/articles/install-pear-pecl-mac-os-x//)
    * 제대로 안됬으면  다시 진행한다.
  * 메시지 확인

```text
gimbohun-ui-MacBook-Pro:~ masterkbh$ sudo pecl channel-update pecl.php.net
Updating channel "pecl.php.net"
Channel "pecl.php.net" is up to date
```

* 다음사항 해결

```text
gimbohun-ui-MacBook-Pro:~ masterkbh$ pecl install xdebug
WARNING: configuration download directory "/tmp/pear/install" is not writeable.  
Change download_dir config variable to a writeable dir to avoid this warning
```

* 한동안 업데이트 안한 OS, xcode 업데이트 이후 에러해결
  * [https://oneboard.tistory.com/21](https://oneboard.tistory.com/21)



참고사이트

* [https://blog.ddoong2.com/2020/05/31/IntelliJ-php-debug/\#](https://blog.ddoong2.com/2020/05/31/IntelliJ-php-debug/#)
* [https://getgrav.org/blog/macos-bigsur-apache-multiple-php-versions](https://getgrav.org/blog/macos-bigsur-apache-multiple-php-versions)
* [https://profilingviewer.com/installing-xdebug-on-bigsur.html](https://profilingviewer.com/installing-xdebug-on-bigsur.html)
* [https://velog.io/@msi753/%EB%A7%A5%EB%B6%81%EC%9C%BC%EB%A1%9C-AMP-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0](https://velog.io/@msi753/%EB%A7%A5%EB%B6%81%EC%9C%BC%EB%A1%9C-AMP-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

