---
title: HTTPS 経由で Docker を使用して ASP.NET Core イメージをホストする
author: rick-anderson
description: HTTPS 経由で Docker を使用して ASP.NET Core イメージをホストする方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: security/docker-https
ms.openlocfilehash: c13ba02845eef5c53a939feec2be8a01bc4ca128
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082530"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a>HTTPS 経由で Docker を使用して ASP.NET Core イメージをホストする

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[既定では、](/aspnet/core/security/enforcing-ssl)ASP.NET Core で HTTPS が使用されます。 [HTTPS](https://en.wikipedia.org/wiki/HTTPS)は、信頼、id、および暗号化のための[証明書](https://en.wikipedia.org/wiki/Public_key_certificate)に依存します。

このドキュメントでは、HTTPS で事前に構築されたコンテナーイメージを実行する方法について説明します。

開発シナリオについては、「 [HTTPS 経由の Docker を使用した ASP.NET Core アプリケーションの開発](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)」を参照してください。

このサンプルでは、docker [17.06](https://docs.docker.com/release-notes/docker-ce)以降の[docker クライアント](https://www.docker.com/products/docker)が必要です。

## <a name="prerequisites"></a>必須コンポーネント

このドキュメントの一部の手順では、 [.Net Core 2.2 SDK](https://www.microsoft.com/net/download)以降が必要です。

## <a name="certificates"></a>証明書

ドメインの[運用ホスト](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/)には、[証明機関](https://en.wikipedia.org/wiki/Certificate_authority)からの証明書が必要です。  [暗号化](https://letsencrypt.org/)は、無料の証明書を提供する証明機関です。

このドキュメントでは、事前に構築さ`localhost`れたイメージをホストするために[自己署名の開発証明書](https://en.wikipedia.org/wiki/Self-signed_certificate)を使用します。 手順は、実稼働証明書の使用に似ています。

実稼働証明書の場合:

* `dotnet dev-certs`ツールは必要ありません。
* 手順で使用した場所に証明書を保存する必要はありません。 任意の場所を使用できますが、証明書をサイトディレクトリ内に格納することはお勧めしません。

指示ボリュームは、コンテナーに証明書をマウントします。 Dockerfile 内のコマンドを使用し`COPY`て、コンテナーイメージに証明書を追加できます。 証明書をイメージにコピーすることはお勧めしません。

* 開発者の証明書を使用したテストで同じイメージを使用するのは困難です。
* 実稼働証明書を使用してホストする場合、同じイメージを使用するのは困難です。
* 証明書の公開には大きなリスクがあります。

## <a name="running-pre-built-container-images-with-https"></a>HTTPS を使用した既成のコンテナーイメージの実行

オペレーティングシステムの構成については、次の手順に従います。

### <a name="windows-using-linux-containers"></a>Linux コンテナーを使用した Windows

証明書を生成してローカルコンピューターを構成する:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

上記のコマンドで、を`{ password here }`パスワードに置き換えます。

HTTPS 用に構成された ASP.NET Core でコンテナーイメージを実行します。

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

パスワードは、証明書に使用されているパスワードと一致している必要があります。

### <a name="macos-or-linux"></a>macOS または Linux

証明書を生成してローカルコンピューターを構成する:

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

`dotnet dev-certs https --trust`は、macOS と Windows でのみサポートされています。 ディストリビューションでサポートされている方法で、Linux 上の証明書を信頼する必要があります。 ブラウザーで証明書を信頼する必要があると考えられます。

上記のコマンドで、を`{ password here }`パスワードに置き換えます。

HTTPS 用に構成された ASP.NET Core でコンテナーイメージを実行します。

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

パスワードは、証明書に使用されているパスワードと一致している必要があります。

### <a name="windows-using-windows-containers"></a>Windows コンテナーを使用した windows

証明書を生成してローカルコンピューターを構成する:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

上記のコマンドで、を`{ password here }`パスワードに置き換えます。

HTTPS 用に構成された ASP.NET Core でコンテナーイメージを実行します。

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

パスワードは、証明書に使用されているパスワードと一致している必要があります。