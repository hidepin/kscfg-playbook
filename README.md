kscfg-playbook
============================================================

# 概要

テンプレートからkickstartファイルを作成するansibleのplaybook

1. apacheのインストール (kscfg_httpd_enableがTrueの場合)
2. apacheの設定 (kscfg_httpd_enableがTrueの場合)
3. apacheの起動 (kscfg_httpd_enableがTrueの場合)
4. kickstartファイルの配置

# 動作確認済み環境

- CentOS6
- CentOS7

# 設定

    kscfg_hostname   # kickstartで作成するマシンのホスト名(ドメイン含む)
    kscfg_hdddev     # kickstartで作成するマシンのHDDのデバイス名
    kscfg_netdev     # kickstartで作成するマシンのNICのデバイス名
    kscfg_gateway    # kickstartで作成するマシンのgatewayアドレス
    kscfg_ip         # kickstartで作成するマシンのIPアドレス
    kscfg_nameserver # kickstartで作成するマシンのnameserverアドレス
    kscfg_netmask    # kickstartで作成するマシンのnetmask
    kscfg_rootpw     # kickstartで作成するマシンのrootパスワード
    kscfg_swapsize   # kickstartで作成するマシンのswapのサイズ(単位MB)

# 依存関係

依存するroleはありません
