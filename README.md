kscfg-playbook
============================================================

# 概要

テンプレートからkickstartファイルを作成するansibleのplaybook

1. apacheのインストール (kscfg_httpd_enableがTrueの場合)
2. apacheの設定 (kscfg_httpd_enableがTrueの場合)
3. apacheの起動 (kscfg_httpd_enableがTrueの場合)
4. kickstartファイルの配置

- UEFI
  インストール時に「e」を押下して下記を設定
  - rhel6/centos6の場合
    - 「vmlinuz initrd=initrd.img」の後ろに、「ks=http://(webサーバ)/kscfg/(ksファイル名) ip=(ipアドレス) netmask=(ネットマスク)」を追加してEnter

  - rhel7/centos7の場合
    - 「linuxefi /images/pxeboot/vmlinuz (略) quiet」の後ろに、「inst.ks=http://(webサーバ)/kscfg/(ksファイル名) ip=(ipアドレス) netmask=(ネットマスク)」を追加してCtrl+x

- BIOS
  インストール時に「tab」を押下して下記を設定
  - rhel6/centos6の場合
    - 「vmlinuz initrd=initrd.img」の後ろに、「ks=http://(webサーバ)/kscfg/(ksファイル名) ip=(ipアドレス) netmask=(ネットマスク)」を追加してEnter

  - rhel7/centos7の場合
    - 「vmlinuz initrd=initrd.img (略) quiet」の後ろに、「inst.ks=http://(webサーバ)/kscfg/(ksファイル名) ip=(ipアドレス) netmask=(ネットマスク)」を追加してEnter
    - メディアチェックを省略した場合は、「rd.live.check」を削除する

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
      - kscfg_template:     : 使用するテンプレートファイル
        - rhel6-small-ks.cfg.j2   : rhel6を最小構成で構築する場合のテンプレートファイル(RHEL6.8のanacondaベース)
        - rhel6-std-ks.cfg.j2     : rhel6を一般的な構成で構築する場合のテンプレートファイル(RHEL6.8のanacondaベース)
        - rhel7-small-ks.cfg.j2   : rhel7を最小構成で構築する場合のテンプレートファイル(RHEL7.3のanacondaベース)
        - rhel7-std-ks.cfg.j2     : rhelを一般的な構成で構築する場合のテンプレートファイル(RHEL7.3のanacondaベース)
        - centos6-small-ks.cfg.j2 : centos6を最小構成で構築する場合のテンプレートファイル(CentOS6.8のanacondaベース)
        - centos6-std-ks.cfg.j2   : centos6を一般的な構成で構築する場合のテンプレートファイル(CentOS6.8のanacondaベース) (検討中)
        - centos7-small-ks.cfg.j2 : centos7を最小構成で構築する場合のテンプレートファイル(CentOS7.2のanacondaベース)
        - centos7-std-ks.cfg.j2   : centosを一般的な構成で構築する場合のテンプレートファイル(CentOS7.2のanacondaベース)
      - kscfg_hostname      : kickstartで作成するマシンのホスト名(ドメイン含む)
      - kscfg_hdddev        : kickstartで作成するマシンのHDDのデバイス名
      - kscfg_netdev        : kickstartで作成するマシンのNICのデバイス名 (RHEL6系の場合linkを指定するとネットワーク設定が正常に完了しない)
      - kscfg_gateway       : kickstartで作成するマシンのgatewayアドレス
      - kscfg_ip            : kickstartで作成するマシンのIPアドレス
      - kscfg_nameserver    : kickstartで作成するマシンのnameserverアドレス (省略可)
      - kscfg_netmask       : kickstartで作成するマシンのnetmask
      - kscfg_rootpw        : kickstartで作成するマシンのrootパスワード(default: root123)
      - kscfg_memsize       : kickstartで作成するマシンのメモリのサイズ(単位MB)
      - kscfg_uefi_firmware : kickstartで作成するマシンのfirmwareがuefiの場合Trueとする (Falseにした場合BIOSモードとして設定する)
      - kscfg_kdump_part    : Trueの場合、memsizeの1.1倍の領域をkdump用に作成する (Falseにした場合kdump領域を確保しない)
      - kscfg_pkg_addlist   : 追加したいパッケージを追加する

# 仕様

  - swapサイズは、kscfg_memsizeの半分の領域を割り当てる
  - kdump領域は、/dumpという名称で、kscfg_memsizeの1.1倍の領域を割り当てる。
  - rhel6-small-ks.cfg.j2のパッケージ
    - 最低限
  - rhel6-std-ks.cfg.j2のパッケージ
    - ソフトウェア開発ワークステーション
      - ベースシステム
        - ネットワーキングツール: 追加チェック
          - 追加パッケージ : wiresharkに追加チェック
      - Webサービス
        - Webサーバ       : 追加チェック
      - システム管理
        - SNMPサポート    : 追加チェック
      - 仮想化
        - (すべてのチェックをはずす)
      - 開発
        - その他の開発    : 追加削除なし
          - 追加パッケージ : postgresql-devel-*のチェックをはずす
      - その他: screen
  - centos6-small-ks.cfg.j2のパッケージ
    - minimal
  - centos6-std-ks.cfg.j2のパッケージ
    - Software Development Workastation
      - ベースシステム
        - ネットワーキングツール: 追加チェック
          - 追加パッケージ : wiresharkに追加チェック
      - Webサービス
        - Webサーバ       : 追加チェック
      - システム管理
        - SNMPサポート    : 追加チェック
      - 仮想化
        - (すべてのチェックをはずす)
      - 開発
        - その他の開発    : 追加削除なし
          - 追加パッケージ : postgresql-devel-*のチェックをはずす
      - その他: screen
  - rhel7-small-ks.cfg.j2のパッケージ
    - minimal
  - rhel7-std-ks.cfg.j2のパッケージ
    - サーバー(GUI使用)
      - 大規模システムのパフォーマンス: 追加チェック
      - パフォーマンスツール        : 追加チェック
      - 開発ツール                 : 追加チェック
      - その他: tmux
  - centos7-small-ks.cfg.j2のパッケージ
    - minimal
  - centos7-std-ks.cfg.j2のパッケージ
    - サーバー(GUI使用)
      - 大規模システムのパフォーマンス: 追加チェック
      - パフォーマンスツール        : 追加チェック
      - 開発ツール                 : 追加チェック
      - その他: tmux

# 依存関係

依存するroleはありません
