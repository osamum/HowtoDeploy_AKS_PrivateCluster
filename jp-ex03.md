# 手順 3: JumpBox から AKS クラスターに接続し、アプリケーションをデプロイする

Azure Kubernetes Service (AKS) Private クラスターは API もちろん、管理接続もプライベート IP アドレスでのみアクセス可能なため、インターネット経由で直接アクセスすることはできません。

よって、AKS Private クラスターを管理するには、同じ仮想ネットワーク内にある JumpBox のデスクトップ環境に接続し、ターミナル画面から Azure CLI を使用するか、同様に JumpBox 上の Web ブラウザーを使用して Azure ポータルにアクセスする必要があります。

この手順では JumpBox から AKS Private クラスターに接続するためのツールをインストールし、AKS Private クラスターに接続してアプリケーションをデプロイする手順を説明します。

## 準備 : JumpBox へのツールのインストール

AKS Private クラスターがデプロイされた仮想ネットワーク上にある、JumpBox に接続し、ターミナル画面を起動して以下のコマンドを実行して、AKS Private クラスターに接続するためのツールをインストールします。

```powershell
# Azure CLI のインストール  
