# Snyk Code & Snyk Open Source ワークショップ

Snyk Code と Snyk Open Source の組み合わせは、使いやすく迅速で、精度の高い SAST と SCA のスキャン機能を提供します。自社で開発したカスタムコード内のセキュリティ問題と、オープンソースパッケージ内の既知の脆弱性、これら両方の検出と修正を、開発とセキュリティの両チームが簡単に実施できます。Snyk を利用することで、セキュリティリスクの対策と開発ペースの向上の両立が実現します。

## 事前準備 (以下をご用意ください)

* GitHub アカウント (パブリックであること) - http://github.com
* git CLI - https://git-scm.com/downloads
* snyk CLI - https://docs.snyk.io/snyk-cli/install-the-snyk-cli ([Snyk CLI のインストールとアップデート](https://qiita.com/ToshiAizawa/items/c090cbd525e45cc5ae51))
* Snyk アカウント - http://app.snyk.io

## このハンズオンワークショップの概要

このハンズオンワークショップでは以下のステップをカバーします。

準備

* [Step 1 - 数多くの脆弱性を含む Juice-Shop アプリケーションのフォーク](#step-1---%E6%95%B0%E5%A4%9A%E3%81%8F%E3%81%AE%E8%84%86%E5%BC%B1%E6%80%A7%E3%82%92%E5%90%AB%E3%82%80-juice-shop-%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E3%83%95%E3%82%A9%E3%83%BC%E3%82%AF)
* [Step 2 - GitHub インテグレーションの設定](#step-2---github-%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E8%A8%AD%E5%AE%9A)

Snyk Code 関連ステップ

* [Step 3 - Snyk UI で Snyk Code の有効化](#step-3---snyk-ui-%E3%81%A7-snyk-code-%E3%81%AE%E6%9C%89%E5%8A%B9%E5%8C%96)
* [Step 4 - プロジェクトの追加 (Snyk Code で脆弱性をスキャン)](#step-4---%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E8%BF%BD%E5%8A%A0-snyk-code-%E3%81%A7%E8%84%86%E5%BC%B1%E6%80%A7%E3%82%92%E3%82%B9%E3%82%AD%E3%83%A3%E3%83%B3)
* [Step 5 - Snyk Code CLI テストの実行](#step-5---snyk-code-cli-%E3%83%86%E3%82%B9%E3%83%88%E3%81%AE%E5%AE%9F%E8%A1%8C)

Snyk Open Source 関連ステップ

* [Step 6 - 脆弱性のスキャン](#step-6---%E8%84%86%E5%BC%B1%E6%80%A7%E3%81%AE%E3%82%B9%E3%82%AD%E3%83%A3%E3%83%B3)
* [Step 7 - PR (プルリクエスト) を通じた修正](#step-7---pr-%E3%83%97%E3%83%AB%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88-%E3%82%92%E9%80%9A%E3%81%98%E3%81%9F%E4%BF%AE%E6%AD%A3)
* [Step 8 - Snyk CLI テストの実行](#step-8---snyk-cli-%E3%83%86%E3%82%B9%E3%83%88%E3%81%AE%E5%AE%9F%E8%A1%8C)

# Workshop Steps

注: 以下のステップでは主に Mac の利用を想定していますが、Windows または Linux 上でもスクリプトを一部変更することで対応できます。

## Step 1 - 数多くの脆弱性を含む Juice-Shop アプリケーションのフォーク

注: Juice-Shop アプリケーションをすでにフォーク済みの場合、このステップは省略できます。次のステップへ進んでください。

次の GitHub リポジトリにアクセスしてください - https://github.com/snyk-japan/juice-shop (注: こちらのリポジトリをフォークした場合、Node.js バージョンとして `10.x`, `12.x`, `14.x` のいずれかをご利用ください)

* "**Fork**" ボタンを選択します
* フォーク先がパブリックな GitHub アカウントであることを確かめます
* "**Create fork**" ボタンを選択します

<img width="1236" alt="image" src="https://user-images.githubusercontent.com/95601557/182498309-a49b75a9-5aa8-4900-a34e-135a84b68666.png">

## Step 2 - GitHub インテグレーションの設定

注: GitHub インテグレーションを設定済みの場合、このステップは省略できます。次のステップへ進んでください。

Snyk を GitHub に接続してリポジトリをインポートできるようにします。以下のステップを実行してください。

* http://app.snyk.io へログインする (サインアップをしていない場合はここでサインアップ)
* Integrations タブ -> Source Control -> GitHub を選択する
* クレデンシャルを設定して GitHub アカウントへ接続する

<img width="1246" alt="Untitled" src="https://user-images.githubusercontent.com/45160975/207766844-dcdfd932-d6b1-46ae-8a0d-62fa01d9072b.png">

# Snyk Code のステップ

Snyk Code はデベロッパーファーストな SAST で開発プロセスに組み込んで利用できるのが特長です。コードを書いた後に問題の検出・修正を行うのではなく、コードを書くタイミングでセキュアなソフトウェア開発を実現します。Snyk は開発者の使う IDE や SCM と連携して動作します。リアルタイムでの問題修正を可能にするために、実用的なスキャン結果を素早く提供します。


## Step 3 - Snyk UI で Snyk Code の有効化

* 画面左の "**Setting**" → "**Snyk Code**" を選択し、`Disabled` (有効化されていない) と表示されている場合は、`Enabled` (有効化されている) に切り替えた後、"**Save Changes**" を選択します

<img width="1881" alt="Untitled 2" src="https://user-images.githubusercontent.com/45160975/207767043-28bb2ded-31c8-42b6-9fd1-0b6f384e0a18.png">


## Step 4 - プロジェクトの追加 (Snyk Code で脆弱性をスキャン)

Snyk が GitHub アカウントと連携したので、フォークされたリポジトリ "**juice-shop**" を Project (プロジェクト) として Snyk にインポートします。

* ページ上部のナビゲーションバーより Projects を選択します
* ページ右上の "**Add Project**" を、続いて "**GitHub**" を選択します
* フォークしたリポジトリ "**juice-shop**" のチェックボックスを選択します
* ページ右上の "**Add Selected Repositories**" ボタンを選択します

![alt tag](https://i.ibb.co/ngxDfvw/Import-Juice-Shop.png)

* インポートが終了すると、"**Code analysis**" プロジェクトが表示されます

![alt tag](https://i.ibb.co/RpScxJ2/Snyk-Code-Results.png)

* "**Code analysis**" を選択して、SAST スキャンの結果を確認します

検出された脆弱性それぞれについて、Snyk は以下の情報を提供します。

* Severity (深刻度)
* 脆弱性の種類
* 該当する CWE (Common Weakness Enumeration: 共通脆弱性タイプ)
* 脆弱性の存在するコードの行
* 脆弱性の概要
* 脆弱性の存在するファイル名
* Snyk Learn モジュールへのリンク (提供されている場合)ー修正方法について学ぶことができます
* 脆弱性を Ignore (無視) する機能ー結果から脆弱性を除外することができます

![alt tag](https://i.ibb.co/yk83tyP/Snyk-Code-vuln.png)

* "**Full details**" ボタンを選択します

Snyk 製品はすべて開発者フレンドリーなエクスペリエンスを提供します。だから Snyk Code で開発者は速やかに問題を把握し、背景を理解した上で、対応方法を学ぶことができます。具体的には、Snyk Code を使ってセキュアではないコードフローをステップ・バイ・ステップで理解できます。検出された脆弱性それぞれについて、ソースファイルのコード行と関連づけて問題を明示します。こうして CWE といった問題を理解しながら、詳しい対処法を学べるのです。

![alt tag](https://i.ibb.co/HnL22t7/Cross-site-scripting-Dataflow.png)


* "**Fix analysis**" を選択します。オープンソースプロジェクトでの実際の修正例を通じて、脆弱性の修正法を確認できます。この画面では、コードの修正例とあわせて、以下の詳細が表示されます (項目は脆弱性の種類によって異なります)。

1. Details (脆弱性についての詳細説明)
1. Prevention (脆弱性の予防法)
1. Types of attacks (派生するアタック種類)
1. Affected environments (影響を受ける環境) 

![alt tag](https://i.ibb.co/M21xScH/Cross-site-scripting-Fix-Analysis.png)

## Step 5 - Snyk Code CLI テストの実行

これまで見てきた Snyk UI に加えて、Snyk は コマンドラインインターフェイス Snyk CLI も提供しています。オープンソースパッケージ、IaC 設定ファイル、ソースコード、それぞれに含まれる脆弱性を検出、修正することができます。 

* この先に進む前に、Snyk CLI の導入が完了していることを確認します。以下の通り、複数のインストール方法を提供しています。ビルド済みバイナリを使えば、NPM のインストールは必要ありません。

1. Install Page - https://docs.snyk.io/snyk-cli/install-the-snyk-cli ([Snyk CLI のインストールとアップデート](https://qiita.com/ToshiAizawa/items/c090cbd525e45cc5ae51))
1. Prebuilt Binaries - https://github.com/snyk/snyk/releases

注: 最新のバージョンをインストールしてください。このワークショップでは以下のバージョンか、それより新しいバージョンが必要です。

```bash
snyk --version
```

```bash
$ snyk --version
1.801.0
```

* 以下のコマンドをを実行して Snyk CLI を認証します

```bash
snyk auth
```

```bash
$ snyk auth

Now redirecting you to our auth page, go ahead and log in,
and once the auth is complete, return to this prompt and you'll
be ready to start using snyk.
[訳: ブラウザ上に認証ページ (Authenticate for CLI) が表示されますので、ログインして認証してください。認証が完了したら (Authenticated) 、コマンドプロンプトに戻ります。Snyk CLI が利用可能になります。]

If you can't wait use this url:
https://snyk.io/login?token=<token>&utm_medium=cli&utm_source=cli&utm_campaign=cli&os=darwin&docker=false

Your account has been authenticated. Snyk is now ready to be used.
```

* 以下のコマンドでリポジトリをローカルにクローンしてください。以下のコマンドの代わりに、ご自身の GitHub にフォークしたリポジトリを使うこともできます。

```bash
git clone https://github.com/juice-shop/juice-shop
```

```bash
$ git clone https://github.com/juice-shop/juice-shop
Cloning into 'juice-shop'...
remote: Enumerating objects: 94967, done.
remote: Counting objects: 100% (17/17), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 94967 (delta 5), reused 13 (delta 4), pack-reused 94950
Receiving objects: 100% (94967/94967), 157.66 MiB | 10.35 MiB/s, done.
Resolving deltas: 100% (72676/72676), done.
```

* カレントディレクトリを "**juice-shop**" に変更します。

```bash
cd juice-shop
```

* "**snyk code test**" コマンドを実行します

```bash
snyk code test
```

```bash
$ snyk code test

Testing /Users/pasapicella/snyk/SE/workshops/SCA-SAST-workshop/juice-shop ...

 ✗ [Low] Cleartext Transmission of Sensitive Information
     Path: test/e2eSubfolder.js, line 7
     Info: http (used in require) is an insecure protocol and should not be used in new code.

 ✗ [Low] Cleartext Transmission of Sensitive Information
     Path: test/api/productReviewApiSpec.js, line 9
     Info: http (used in require) is an insecure protocol and should not be used in new code.

...

 ✗ [High] Server-Side Request Forgery (SSRF)
     Path: routes/profileImageUrlUpload.js, line 19
     Info: Unsanitized input from the HTTP request body flows into request.get, where it is used as an URL to perform a request. This may result in a Server-Side Request Forgery vulnerability.

 ✗ [High] Server-Side Request Forgery (SSRF)
     Path: frontend/src/app/Services/configuration.service.ts, line 111
     Info: Unsanitized input from data from a remote resource flows into get, where it is used as an URL to perform a request. This may result in a Server-Side Request Forgery vulnerability.

 ✗ [High] Improper Neutralization of Directives in Statically Saved Code
     Path: routes/userProfile.js, line 46
     Info: Unsanitized input from cookies flows into pug.compile, where it is used to construct a template that gets rendered. This may result in a Server-Side Template Injection vulnerability.


✔ Test completed

Organization:      undefined
Test type:         Static code analysis
Project path:      /Users/pasapicella/snyk/SE/workshops/SCA-SAST-workshop/juice-shop

199 Code issues found
25 [High]  17 [Medium]  157 [Low]

```

### さらに Snyk Code を試す - Snyk Code ワークショップ

さらに Snyk Code を試す場合は、このワークショップ https://github.com/papicella/snyk-code-workshop に取り組んでみてください。追加のステップがあります。(Snyk Code CLI Test、Snyk Code Test using VS Code)

# Snyk Open Source のステップ

Snyk Open Source は SCA ツール (SCA: Software Composition Analysis=ソフトウェア構成解析) です。オープンソースパッケージに由来する脆弱性とライセンスポリシー違反を検出、修正、管理することができます。

## Step 6 - 脆弱性のスキャン

* Juice-Shop プロジェクトは Step 3 でインポートされているので、Snyk UI にて "**package.json**" が表示されているはずです。

![alt tag](https://i.ibb.co/d4Qb3TV/Snyk-OS-vuln.png)

* "**package.json**" (複数ある場合は `frontend/` ではないもの) を選択して、オープンソースのスキャン結果を確認します

Juice-Shop プロジェクトのリスクを確認しましょう。オープンソースの依存パッケージを宣言しているマニフェストファイル "**package.json**" を選択してください。マニフェストファイルの内容を確認することができます。

![alt tag](https://i.ibb.co/ZhN6tXY/Package-json-view.png)

続いて Snyk UI へ戻り、脆弱性を確認します。

脆弱性はデフォルトでは [Priority Score](https://docs.snyk.io/features/fixing-and-prioritizing-issues/starting-to-fix-vulnerabilities/snyk-priority-score) 順でソートされます。それぞれの脆弱性に対して、以下の情報が提供されます。

1. Introduced through: 脆弱性の混入元であるパッケージ (推移的依存パッケージからの混入の場合は、マニフェストファイルで宣言されたパッケージが表示されます)
1. Detailed paths and remediations: 混入元の依存パッケージまでのパスと、修正方法
1. Overview: 脆弱性についての概要
1. Exploit maturity: 攻撃可能性 (PoC=実証コードの有無など、攻撃される可能性の程度)
1. Links to CWE, CVE and CVSS Score: CWE と CVE へのリンク、CVSS スコア
1. その他

![alt tag](https://i.ibb.co/xq2GWCs/Snyk-OS-vuln.png)

## Step 7 - PR (プルリクエスト) を通じた修正

GitHub インテグレーションを使用していて、かつ修正が存在する場合、Snyk は Pull Request (プルリクエスト、以下 PR) を通じて、脆弱性のある依存パッケージを修正済みバージョンへ自動アップグレードすることができます。

* "**Prototype Pollution**" の脆弱性について、"**Fix this vulnerability**" ボタンを選択します

![alt tag](https://i.ibb.co/9NHPmn2/Snyk-OS-Fix-this-vuln.png)

* 次の画面で、PR で修正する脆弱性を確認することができます。画面下部の "**Open a Fix PR**" ボタンを選択します

![alt tag](https://i.ibb.co/y5PHhhT/Vulns-to-fix-pr-view.png)
![alt tag](https://i.ibb.co/p2Lx5Rd/Open-fix-pr-button.png)

* 準備ができると GitHub 上で PR が表示されます。ここでは Files changed タブから diff (変更による差分) を確認することができます。

Snyk はお好みの Git リポジトリと連携して、PR の作成時にマニフェストファイルをスキャン可能です。メインブランチにマージする前の段階で、コードをセキュアな状態に維持することができます。

![alt tag](https://i.ibb.co/ySc72zN/Fix-PR-Github.png)
![alt tag](https://i.ibb.co/BzrNHvg/Files-changed-Github.png)

* セキュリティチェックが行われ、この PR によって新たな脆弱性が発生しないことを確認できます
* (任意) PR をマージします
* PR をマージした場合、Snyk UI へ戻って脆弱性が修正されていることを確認することもできます

### Step 8 - Snyk CLI テストの実行

これまで見てきた Snyk UI に加えて、Snyk は コマンドラインインターフェイス Snyk CLI も提供しています。これによりオープンソースパッケージに含まれる脆弱性を検出、修正することができます。 この CLI を DevOps パイプラインに組み込むことで、アプリケーションを本番にデプロイするワークフロー内でアプリケーションセキュリティのスキャンを実行することもできます。

* Snyk CLI でのスキャン実行には npm パッケージのインストールが必要です。npm がインストール済みであれば、以下のコマンドで npm パッケージのインストールを行います。

```bash
npm install
```

注: npm がインストールされていない場合、このステップをスキップして構いません。**package-lock.json** ファイル、もしくは、"**node_modules**" フォルダがない状態では `snyk test` コマンドは実行できません。

* npm パッケージがインストールされたら、Snyk CLI を実行します

```bash
snyk test --all-projects
```

```bash
$ snyk test --all-projects

Testing /Users/pasapicella/snyk/SE/workshops/SCA-SAST-workshop/juice-shop...

Tested 898 dependencies for known issues, found 37 issues, 52 vulnerable paths.


Issues to fix by upgrading:

  Upgrade concurrently@5.3.0 to concurrently@6.0.0 to fix
  ✗ Regular Expression Denial of Service (ReDoS) [High Severity][https://snyk.io/vuln/SNYK-JS-ANSIREGEX-1583908] in ansi-regex@4.1.0
    introduced by concurrently@5.3.0 > yargs@13.3.2 > string-width@3.1.0 > strip-ansi@5.2.0 > ansi-regex@4.1.0 and 8 other path(s)

  Upgrade express-jwt@0.1.3 to express-jwt@6.0.0 to fix
  ✗ Regular Expression Denial of Service (ReDoS) [Low Severity][https://snyk.io/vuln/npm:moment:20170905] in moment@2.0.0
    introduced by express-jwt@0.1.3 > jsonwebtoken@0.1.0 > moment@2.0.0
    
...

  ✗ Use After Free [High Severity][https://snyk.io/vuln/SNYK-JS-NODESASS-541000] in node-sass@4.14.1
    introduced by node-sass@4.14.1
  No upgrade or patch available
  ✗ Out-of-bounds Read [Medium Severity][https://snyk.io/vuln/SNYK-JS-NODESASS-541002] in node-sass@4.14.1
    introduced by node-sass@4.14.1
  No upgrade or patch available


License issues:

  ✗ GPL-3.0 license (new) [High Severity][https://snyk.io/vuln/snyk:lic:npm:qrious:GPL-3.0] in qrious@4.0.2
    introduced by anuglar2-qrcode@2.0.9998 > qrious@4.0.2



Organization:      pas.apicella-41p
Package manager:   npm
Target file:       frontend/package.json
Project name:      frontend
Open source:       no
Project path:      /Users/pasapicella/snyk/SE/workshops/SCA-SAST-workshop/juice-shop
Licenses:          enabled


Tested 2 projects, 2 contained vulnerable paths.

```

### さらに Snyk Open Source を試す - Snyk Open Source ワークショップ

さらに Snyk Open Source を試す場合は、このワークショップ https://github.com/papicella/snyk-open-source-workshop に取り組んでみてください。追加のステップがあります。(IDE Integration with VS Code, etc.)


以上でワークショップ完了です。お疲れさまでした！
本日はご参加ありがとうございました。

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Jenny Granja [jennifer.granja at snyk.io] is a Solutions Engineer at Snyk APJ 
<br/>
Pas Apicella [pas at snyk.io] is a Principal Solutions Engineer at Snyk APJ
<br/>
Toshi Aizawa [toshi.aizawa at snyk.io] is a Senior Solutions Engineer at Snyk APJ

