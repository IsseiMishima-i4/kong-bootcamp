# kong-gateway

# このリポジトリについて
Kong API Gatewayを使用したAPI管理について体系的に学習することができます。
本レポジトリではKong API GatewayのService,Route,Pluguin、デモアプリケーション、CICD、監視,監査ログ、APIOpsなどの資材を用意しています。

# 構築手順
0. 事前準備<br>
1. Kongイメージ,KongDataPlaneの作成<br>
2. BookInfoアプリのデプロイ<br>
3. 監視ツールのデプロイ<br>
4. 監査ログの取得<br>
5. APIOpsの実装br>

# 0.事前準備
1. Kubernetes環境の準備
2. deck,helm,kubectl,curlコマンドのインストール
3. Kubernetes環境からインターネットへアウトバウンド通信が可能な状態にする
4. Kong Konnectアカウント及びKongControlPlaneの準備

# 1.Kongイメージ,KongDataPlaneの作成
今回はDockerHubで提供されているコンテナイメージを自前のレポジトリにpushしスキャンして脆弱性確認を実施するというワークフローと、そのイメージをもとにKongDataPlaneをデプロイするというワークフローの２つを作成している。

1. .github/workflows/docerhub2ghr.yamlで定義されているワークフローをもとにGithubActionsを実行する
2. .github/workflows/deploy_dp.yamlを実行する

# 2.BookInfoアプリのデプロイ
BookInfoのアプリの資材とアプリをインターネット公開するためのIngress(contour)の資材をデプロイする

1. bookinfo/platform/kube/bookinfo.yamlをデプロイ
2. bookinfo/platform/kube/bookinfo-ingressをデプロイ
3. ProductPageにアクセスできることを確認

# 3.監視ツールのデプロイ
今回は監視ツールはPrometheus,Grafanaを使用する

1. https://qiita.com/ipppppei/items/c15acc5c7f7af3e7c289を参照してPrometheus,Grafanaをデプロイ
2. Prometheus,Grafanaにアクセスできることを確認

# 4.監査ログの取得
監査ログを取得・出力するためのロジックを記載したConfigMapとPodをデプロイする
また、今回は監査ログがPodに出力されるところまでの実装とし、監査ログの保存まではスコープとしていない(商用利用の際は監査ログは保存できる状態にしておいた方が望ましいだろう)

1. auditlogs/auditlogs.yamlのデプロイ
2. auditlogs/auditlogs_configmap.yamlのデプロイ
3. Kongにて監査ログを有効化する
4. 1で立てたPodのログを出力、監査ログが出力されていることを確認

# 5.APIOpsの実装
APIの仕様が変更されAPISpec及びProductドキュメントが更新された際に、自動でKongのDevPortalにアップロード及びe2eテストがなされるようにGithubActionsでの実装を行なっている

1. docs/product.mdまたはdocs/openapi/api-spec.yamlに変更を加える
2. .github/workflows/deploy_oas.yaml,.github/workflows/upload_spec.yamlまたはgithub/workflows/upload_doc.yamlが自動で実行される
3. Kong DevPortalを確認し変更が加えられていることを確認
