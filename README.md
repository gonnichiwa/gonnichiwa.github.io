# Install This Blog on windows

## prerequisites
1. setup git
1. source clone

## download ruby
1. Ruby+Devkit 3.3.1-1 (x64) [download](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-3.3.1-1/rubyinstaller-devkit-3.3.1-1-x64.exe)

## install file & setup on cmd
1. __setup ruby + mingw devkit__
  - __다운로드 파일 setup : 1 선택__
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBxyQF%2FbtsF015IF1r%2F1gMY62LVo2pzoNFv4J2Gs1%2Fimg.png)

1. __setup jekyll env__
> - jekyll env setup doc : [__jekyll on windows__](https://jekyllrb.com/docs/installation/windows/)
> - ```clonedpath\> gem install jekyll bundler ```
> - ```clonedpath\> bundle install ```

## start blog on local env
1. open new `cmd` on __administrator permission__
2. ``` \> bundle exec jekyll serve ```
> 구동 확인
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FciANyq%2FbtsF2xbjBfA%2Fz1nCdUEvjPCqPh83vVJvB1%2Fimg.png)

3. 브라우저에서 http://localhost:4000 진입.

<br/>

---

<br/>

## Based On Chirpy Starter

[![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy)][gem]&nbsp;
[![GitHub license](https://img.shields.io/github/license/cotes2020/chirpy-starter.svg?color=blue)][mit]

When installing the [**Chirpy**][chirpy] theme through [RubyGems.org][gem], Jekyll can only read files in the folders
`_data`, `_layouts`, `_includes`, `_sass` and `assets`, as well as a small part of options of the `_config.yml` file
from the theme's gem. If you have ever installed this theme gem, you can use the command
`bundle info --path jekyll-theme-chirpy` to locate these files.

The Jekyll team claims that this is to leave the ball in the user’s court, but this also results in users not being
able to enjoy the out-of-the-box experience when using feature-rich themes.

To fully use all the features of **Chirpy**, you need to copy the other critical files from the theme's gem to your
Jekyll site. The following is a list of targets:

```shell
.
├── _config.yml
├── _plugins
├── _tabs
└── index.html
```

To save you time, and also in case you lose some files while copying, we extract those files/configurations of the
latest version of the **Chirpy** theme and the [CD][CD] workflow to here, so that you can start writing in minutes.

## Prerequisites

Follow the instructions in the [Jekyll Docs](https://jekyllrb.com/docs/installation/) to complete the installation of
the basic environment. [Git](https://git-scm.com/) also needs to be installed.

## Installation

Sign in to GitHub and [**use this template**][use-template] to generate a brand new repository and name it
`USERNAME.github.io`, where `USERNAME` represents your GitHub username.

Then clone it to your local machine and run:

```console
$ bundle
```

## Usage

Please see the [theme's docs](https://github.com/cotes2020/jekyll-theme-chirpy#documentation).

## License

This work is published under [MIT][mit] License.

[gem]: https://rubygems.org/gems/jekyll-theme-chirpy
[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy/
[use-template]: https://github.com/cotes2020/chirpy-starter/generate
[CD]: https://en.wikipedia.org/wiki/Continuous_deployment
[mit]: https://github.com/cotes2020/chirpy-starter/blob/master/LICENSE
