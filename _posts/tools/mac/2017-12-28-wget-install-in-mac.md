---
layout: post
title:  "맥 (os X)에 wget 설치하기"
categories: mac
tags: mac wget
---

## `wget`이란?

"wget은 `GNU Wget`의 줄임말로, 웹 서버에서 콘텐츠를 가져오는 컴퓨터 프로그램으로 GNU 프로젝트의 일부이다. 이 프로그램의 이름은 WWW(world wide web)과 get에서 가져온 것이다. HTTP, HTTPS, FTP 프로토콜을 통해 내려받기를 지원한다." 
(출처: [wget 위키백과](https://ko.wikipedia.org/wiki/Wget))

## `wget` 설치하기
맥에는 wget이 설치되어 있지 않기 때문에 따로 설치를 해 주어야 한다.

여러 가지 방법으로 설치를 할 수 있는데, `brew`를 이용한 방법과 `curl`로 최신 소스를 가져와 직접 설치하는 방법을 소개한다.


### `brew`로 설치
모든 brew 설치가 그렇듯이, 명령어 한 줄로 아주 간단하게 설치할 수 있다.

```bash
$brew install wget

Updating Homebrew...
==> Auto-updated Homebrew!
Updated Homebrew from b4d43e950 to 2259e369e.
Updated 1 tap (homebrew/core).
==> New Formulae
bamtools       cp2k           gox            samtools       spades         stress-ng      tnftp          travis         yq
bcftools       diamond        lammps         seqtk          sratoolkit     telnetd        tnftpd         vcftools
==> Updated Formulae
python3 ✔                  diffoscope                 gspell                     mongo-c-driver             ruby@1.9
ruby ✔                     dlib                       gtkspell3                  mongodb                    ruby@2.0
abcm2ps                    docfx                      guile                      mpd                        ruby@2.1
abcmidi                    docker-compose             guile@2.0                  nats-streaming-server      ruby@2.2
abyss                      docker-compose-completion  hadolint                   ncmpcpp                    ruby@2.3
ack                        ecl                        haproxy                    nco                        rustup-init
algernon                   elasticsearch              harfbuzz                   neko                       sagittarius-scheme
allure                     elixir                     haskell-stack              newsboat                   sip
amazon-ecs-cli             emscripten                 haste-client               nghttp2                    sjk
angband                    enchant                    heartbeat                  nnn                        snakemake
angular-cli                erlang                     heroku                     nomad                      snappy
ansible-lint               etcd                       influxdb@0.8               nq                         source-highlight
apache-opennlp             faad2                      jena                       opencv                     spdlog
apktool                    fabio                      jenkins                    opencv@2                   sqlcipher
apm-server                 fbi-servefiles             jsoncpp                    openttd                    squashfs
app-engine-java            fftw                       kibana                     osm2pgrouting              sslyze
arangodb                   filebeat                   kite                       osquery                    statik
argyll-cms                 fio                        kontena                    osrm-backend               stubby
armor                      fish                       latexila                   packetbeat                 syncthing
asdf                       fluent-bit                 ledger                     paket                      terraform_landscape
autogen                    fmt                        lgogdownloader             pandoc-citeproc            texmath
azure-cli                  fn                         libatomic_ops              parallel                   tig
baresip                    folly                      libbitcoin                 pcl                        tile38
bash-git-prompt            fonttools                  libbitcoin-blockchain      pcsc-lite                  tth
bazel                      frugal                     libbitcoin-database        pdftoedn                   ttyrec
bdw-gc                     gauge                      libbitcoin-explorer        percona-server             twarc
bettercap                  gdcm                       libbitcoin-node            pgbouncer                  uhd
boost                      gedit                      libbitcoin-server          pgcli                      unoconv
boost-bcp                  getdns                     libcouchbase               picard-tools               urh
boost-build                getmail                    libfabric                  picocom                    vault
boost-mpi                  gexiv2                     libpqxx                    pipenv                     vim
boost-python               ghi                        libressl                   pjproject                  vim@7.4
cayley                     git                        libstfl                    ponyc                      vtk
cgrep                      gitbucket                  llvm                       pre-commit                 w3m
chamber                    gitg                       logstash                   protobuf                   weechat
cheat                      github-keygen              lxc                        pypy                       wesnoth
chronograf                 gitlab-runner              lysp                       pypy3                      widelands
cimg                       gmime                      macvim                     qmmp                       winetricks
conjure-up                 gnome-builder              mariadb@10.1               qpid-proton                wireguard-tools
convox                     gnome-recipes              media-info                 quicktype                  wolfssl
corebird                   gnu-tar                    mediaconch                 rabbitmq                   wpscan
corsixth                   gnumeric                   memcached                  radamsa                    yle-dl
cppad                      gnupg                      menhir                     radare2                    you-get
creduce                    gnuradio                   metaproxy                  rclone                     youtube-dl
cromwell                   gollum                     metricbeat                 rebar@3                    z3
crystal-icr                gopass                     mikutter                   rocksdb                    zile
crystal-lang               gradle                     miniupnpc                  root                       zstd
cucumber-cpp               grpc                       mkvtoolnix                 roswell
dar                        gsoap                      moco                       ruby-build
==> Renamed Formulae
camlistore -> perkeep
==> Deleted Formulae
mongodb@2.6                                  ponscripter-sekai                            stklos

==> Installing dependencies for wget: libunistring, libidn2
==> Installing wget dependency: libunistring
==> Downloading https://homebrew.bintray.com/bottles/libunistring-0.9.8.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libunistring-0.9.8.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libunistring/0.9.8: 53 files, 4.4MB
==> Installing wget dependency: libidn2
==> Downloading https://homebrew.bintray.com/bottles/libidn2-2.0.4.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libidn2-2.0.4.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libidn2/2.0.4: 46 files, 580.8KB
==> Installing wget
==> Downloading https://homebrew.bintray.com/bottles/wget-1.19.2_1.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring wget-1.19.2_1.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/wget/1.19.2_1: 50 files, 3.8MB
```
위처럼 결과가 나와야 정상적으로 설치 된 것.


### 최신 소스를 다운받아 직접 설치
손이 많이 가고, linux 명령어에 대한 기본 지식이 있어야 할 듯 싶다. 위 `brew` 명령어가 잘 안 되거나, 에러가 났을 경우 이 방법을 사용해서 설치하면 된다.

#### 1. wget을 직접 다운로드
[wget 공식 페이지](https://www.gnu.org/software/wget/)에 가면 wget을 다운로드 받을 수 있다.

[여기](https://ftp.gnu.org/gnu/wget/)서 버전별 wget을 확인하고 원하는 버전을 다운받을 수 있다. `2017-12-28` 기준 `wget-1.19`가 최신 버전이므로 1.19 버전을 눌러서 직접 브라우저에서 다운로드 하거나, 링크를 복사하여 `curl` 명령어를 사용해서 다운로드 받아도 된다.


```bash
$curl -O https://ftp.gnu.org/gnu/wget/wget-1.19.tar.xz
```

> `curl -O` 는 `--remote-name`의 약자로 해당 파일을 remote name과 동일한 이름으로 다운로드받는다.


#### 2. tar 파일 압축 풀기
해당 파일을 다운로드 받은 폴더로 이동한 후, (`curl` 명령어를 실행한 위치) 합축을 풀어준다.

```bash
$tar -zxvf wget-1.19.tar.xz
```

#### 3. 압축을 푼 directory로 이동
```bash
$cd wget-1.19
```

#### 4. wget 폴더 안에서 환경설정
```bash
$./configure -with-ssl=openssl
```
> 뒤의 `-with-ssl=openssl`은 wget의 `GNUTILS`가 맥 os에서 지원되지 않으므로 `OpenSSL`로 대체

#### 5. make 명령어로 설치
```bash
$make && make install
```
> `&&`은 해당 명령어가 완료되어야 다음 명령어를 실행하라는 코맨드. `&`는 이전 명령어가 성공하지 않더라도 다음 명령어를 실행한다.
