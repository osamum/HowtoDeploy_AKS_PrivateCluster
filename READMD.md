# Azure の仮想ネットワークに閉域化された Azure Kubernetes Service (AKS) を構築する

Azure の仮想ネットワークのリソースにのみサービスを提供する Azure Kubernetes Service (AKS) クラスターを構築するための手順を説明します。

この手順で構築された AKS クラスターは、インターネットから直接アクセスできないように閉域化されます。これにより、セキュリティが強化され、内部ネットワーク内のリソースとの通信のみが可能になります。

ただし、AKS クラスターからのインターネットへのアクセスは可能とし、アプリケーションのコンテナイメージは GitHub Container Registry (GHCR) などから取得できるように構成します。

![VNET で閉域化された AKS のシステム構成図](img/Private_AKS_SystemArchtecture.png)

# 概要

オンプレミス環境から運用中のシステムを Microaoft Azure に移行する際、多くの場合、セキュリティの観点からインターネットに直接接続できない閉域化された環境での運用が求められます。Azure Kubernetes Service (AKS) は、マネージドな Kubernetes クラスターを提供するサービスであり、仮想ネットワーク内での閉域化された構成をサポートしています。

この手順ではすでに [Azure 上に仮想ネットワークと Jumpbox (踏み台サーバー)が構築されている](https://github.com/osamum/HowtoMake-Az-JumpBox-Env)ことを前提とし、その既存の仮想ネットワーク内に AKS クラスターを構築する方法を説明します。AKS クラスターは、仮想ネットワーク内のリソースにのみアクセス可能であり、インターネットからの直接アクセスは制限されます。

<br>

# 前提条件

この手順を実施する前に、以下の前提条件を満たしている必要があります。

* Azure サブスクリプションが有効であること
* Azure ポータルにアクセスできること
* Azure の管理者権限か共同作成者の権限を持っていること
* 以下の手順で構築された仮想ネットワークと Jumpbox が存在すること
  - [Azure の仮想ネットワークで閉域化された環境に安全にアクセスするための Jump Box 環境の構築](https://github.com/osamum/HowtoMake-Az-JumpBox-Env)

# この手順で構築される環境

既存の Azure 仮想ネットワーク内にインターネットからアクセス不能な Azure Kubernetes Service (AKS) プライベート クラスターを構築します。

また、AKS クラスターを構築するだけでなく、実際に GitHub Container Registry (GHCR) から [AKS Store Demo](https://github.com/Azure-Samples/aks-store-demo) のコンテナーイメージを取得してアプリケーションをデプロイして動作させるまでの手順を説明します。

# 手順

1. [既存の Azure 仮想ネットワークに AKS 用のサブネットを追加](jp-ex01.md)
2. [Azure Kubernetes Service (AKS) Private クラスターの構築](jp-ex02.md)
3. [JumpBox から AKS クラスターに接続し、アプリケーションをデプロイする](jp-ex03.md)

<br>





