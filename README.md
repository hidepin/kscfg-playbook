kscfg-playbook
============================================================

# 概要

テンプレートからkickstartファイルを作成するansibleのplaybook

1. apacheのインストール (kscfg_httpd_enableがTrueの場合)
2. apacheの設定 (kscfg_httpd_enableがTrueの場合)
3. apacheの起動 (kscfg_httpd_enableがTrueの場合)
4. kickstartファイルの配置

インストール時に「tab」を押下して下記を設定
  - rhel6の場合
    - 「vmlinuz initrd=initrd.img」の後ろに、ks=http://(webサーバ)/kscfg/(ksファイル名) ip=(ipアドレス) netmask=(ネットマスク)

# 動作確認済み環境

- RHEL6
- RHEL7
- CentOS6
- CentOS7

# 設定

  - 全体設定
      - kscfg_httpd_enable: Trueの場合Apacheの設定を行う
      - kscfg_path        : kickstartの設定ファイルを配置するディレクトリ
  - kscfg設定
      - kscfg_template:   : 使用するテンプレートファイル
        - rhel6-small-ks.cfg.j2     : rhel6を最小構成で構築する場合のテンプレートファイル(RHEL6.8のanacondaベース)
        - rhel6-default-ks.cfg.j2   : rhel6を一般的な構成で構築する場合のテンプレートファイル(RHEL6.8のanacondaベース)
        - rhel7-small-ks.cfg.j2     : rhel7を最小構成で構築する場合のテンプレートファイル(RHEL7.2のanacondaベース)
        - rhel7-default-ks.cfg.j2   : rhelを一般的な構成で構築する場合のテンプレートファイル(RHEL7.2のanacondaベース)
        - centos6-small-ks.cfg.j2   : centos6を最小構成で構築する場合のテンプレートファイル(CentOS6.8のanacondaベース)
        - centos6-default-ks.cfg.j2 : centos6を一般的な構成で構築する場合のテンプレートファイル(CentOS6.8のanacondaベース)
        - centos7-small-ks.cfg.j2   : centos7を最小構成で構築する場合のテンプレートファイル(CentOS7.2のanacondaベース)
        - centos7-default-ks.cfg.j2 : centosを一般的な構成で構築する場合のテンプレートファイル(CentOS7.2のanacondaベース)
      - kscfg_hostname    : kickstartで作成するマシンのホスト名(ドメイン含む)
      - kscfg_hdddev      : kickstartで作成するマシンのHDDのデバイス名
      - kscfg_netdev      : kickstartで作成するマシンのNICのデバイス名
      - kscfg_gateway     : kickstartで作成するマシンのgatewayアドレス
      - kscfg_ip          : kickstartで作成するマシンのIPアドレス
      - kscfg_nameserver  : kickstartで作成するマシンのnameserverアドレス
      - kscfg_netmask     : kickstartで作成するマシンのnetmask
      - kscfg_rootpw      : kickstartで作成するマシンのrootパスワード
      - kscfg_memsize     : kickstartで作成するマシンのメモリのサイズ(単位MB)
      - kscfg_kdump_part  : Trueの場合、memsizeの1.1倍の領域をkdump用に作成する

# 仕様

  - swapサイズは、kscfg_memsizeの半分の領域を割り当てる
  - kdump領域は、/dumpという名称で、kscfg_memsizeの1.1倍の領域を割り当てる。

# 依存関係

依存するroleはありません
