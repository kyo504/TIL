## Xcode 에서 node 를 찾지 못해 빌드가 오류나는 문제

NVM 을 사용하는 환경에서 Xcode 로 빌드할 때 node 를 찾지 못해서 아래와 같은 에러가 나는 경우가 있다.

```
env: node: No such file or directory
```

이유는 Xcode 에서 찾는 node의 path 가 `/usr/local/bin` 로 되어 있기 때문이다.

문제를 해결하기 위해서는 아래와 같이 스크립트를 만들고,

`sudo vi /usr/local/bin/node`

아래의 내용을 채워서 저장하고,

```
#!/usr/bin/env sh
# Use the version of node specified in .nvmrc to run the supplied command

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

nvm_rc_version > /dev/null 2> /dev/null
HAS_NVM_RC=$?

if [[ "$HAS_NVM_RC" == "0" ]] ; then
    nvm run $*
else 
    nvm run default $*
fi
exit $?
```

파일이 실행가능하도록 권한을 수정한다.

`sudo chmod +x /usr/local/bin/node`

### Reference
- https://blog.nigelsim.org/2022-12-09-nvm-node-from-xcode/
