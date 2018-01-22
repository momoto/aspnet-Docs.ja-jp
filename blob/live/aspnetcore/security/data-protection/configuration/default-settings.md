---
title: "データ保護キーの管理および ASP.NET Core の有効期間"
author: rick-anderson
description: "データ保護キーの管理および ASP.NET Core の有効期間について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 0993c68e37944a3aad863b98f92fe0140cfb746d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a><span data-ttu-id="d8309-103">データ保護キーの管理および ASP.NET Core の有効期間</span><span class="sxs-lookup"><span data-stu-id="d8309-103">Data Protection key management and lifetime in ASP.NET Core</span></span>

<span data-ttu-id="d8309-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8309-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="key-management"></a><span data-ttu-id="d8309-105">キー管理</span><span class="sxs-lookup"><span data-stu-id="d8309-105">Key management</span></span>

<span data-ttu-id="d8309-106">アプリは、運用環境を検出し、独自のキーの構成の処理を試行します。</span><span class="sxs-lookup"><span data-stu-id="d8309-106">The app attempts to detect its operational environment and handle key configuration on its own.</span></span>

1. <span data-ttu-id="d8309-107">アプリがホストされている場合[Azure アプリ](https://azure.microsoft.com/services/app-service/)、キーに永続化、 *%HOME%\ASP.NET\DataProtection-Keys*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8309-107">If the app is hosted in [Azure Apps](https://azure.microsoft.com/services/app-service/), keys are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="d8309-108">このフォルダーはネットワーク ストレージによってバックアップされは、アプリをホストしているすべてのマシン間で同期します。</span><span class="sxs-lookup"><span data-stu-id="d8309-108">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span>
   * <span data-ttu-id="d8309-109">キーは、残りの部分で保護されていません。</span><span class="sxs-lookup"><span data-stu-id="d8309-109">Keys aren't protected at rest.</span></span>
   * <span data-ttu-id="d8309-110">*データ保護キー*フォルダーは、1 つのデプロイ スロットで、アプリのすべてのインスタンスにキー リングを提供します。</span><span class="sxs-lookup"><span data-stu-id="d8309-110">The *DataProtection-Keys* folder supplies the key ring to all instances of an app in a single deployment slot.</span></span>
   * <span data-ttu-id="d8309-111">ステージングと運用環境などの別のデプロイ スロットには、キーのリングを共有しません。</span><span class="sxs-lookup"><span data-stu-id="d8309-111">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span> <span data-ttu-id="d8309-112">たとえばステージング、実稼働のスワップまたは A を使用して、デプロイ スロット間で交換する場合に/テスト B、すべてのアプリ データ保護を使用して、前のスロット内のキーのリングを使用して格納されたデータの暗号化を解除することはできません。</span><span class="sxs-lookup"><span data-stu-id="d8309-112">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any app using Data Protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="d8309-113">これはそのクッキーを保護するデータの保護を使用するように、標準の ASP.NET Core の cookie 認証を使用するアプリからログアウトされているユーザーにつながります。</span><span class="sxs-lookup"><span data-stu-id="d8309-113">This leads to users being logged out of an app that uses the standard ASP.NET Core cookie authentication, as it uses Data Protection to protect its cookies.</span></span> <span data-ttu-id="d8309-114">スロットに依存しないキーリングを希望する場合は、Azure Blob ストレージ、Azure Key Vault SQL ストアなど、外部キー リング プロバイダーを使用または Redis キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="d8309-114">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

1. <span data-ttu-id="d8309-115">キーが永続化、ユーザー プロファイルを使用できる場合、 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8309-115">If the user profile is available, keys are persisted to the *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="d8309-116">オペレーティング システムが Windows DPAPI を使用して、キーが暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="d8309-116">If the operating system is Windows, the keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="d8309-117">アプリが IIS でホストされている場合、キーは、ワーカー プロセス アカウントにのみ ACLed である特殊なレジストリ キー HKLM レジストリに保存されます。</span><span class="sxs-lookup"><span data-stu-id="d8309-117">If the app is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that is ACLed only to the worker process account.</span></span> <span data-ttu-id="d8309-118">キーは DPAPI を使用して保存時に暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="d8309-118">Keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="d8309-119">これらの条件のいずれも一致しない場合、キーは、現在のプロセスの外部で永続化されます。</span><span class="sxs-lookup"><span data-stu-id="d8309-119">If none of these conditions match, keys aren't persisted outside of the current process.</span></span> <span data-ttu-id="d8309-120">プロセスがシャット ダウン、生成されたすべてのキーが失われます。</span><span class="sxs-lookup"><span data-stu-id="d8309-120">When the process shuts down, all generated keys are lost.</span></span>

<span data-ttu-id="d8309-121">開発者は常にフル コントロールであり、キーの格納場所と方法をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="d8309-121">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="d8309-122">上記の最初の 3 つのオプションは、ほとんどのアプリ方法と似ています適切な既定値を提供する必要があります、ASP.NET  **\<machineKey >**過去で自動生成ルーチンが機能していた。</span><span class="sxs-lookup"><span data-stu-id="d8309-122">The first three options above should provide good defaults for most apps similar to how the ASP.NET **\<machineKey>** auto-generation routines worked in the past.</span></span> <span data-ttu-id="d8309-123">最後に、フォールバック オプションを指定する、開発者が必要な唯一のシナリオは、[構成](xref:security/data-protection/configuration/overview)キーの永続化したいが、このフォールバックはまれな状況でのみ発生する場合は、先行します。</span><span class="sxs-lookup"><span data-stu-id="d8309-123">The final, fallback option is the only scenario that requires the developer to specify [configuration](xref:security/data-protection/configuration/overview) upfront if they want key persistence, but this fallback only occurs in rare situations.</span></span>

<span data-ttu-id="d8309-124">Docker ボリュームの共有ボリューム (ホストでマウントされたボリューム コンテナーの有効期間を超えて保持) であるフォルダーでキーを保持するか、Docker コンテナーでホストしている、または外部プロバイダーのように[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)です。</span><span class="sxs-lookup"><span data-stu-id="d8309-124">When hosting in a Docker container, keys should be persisted in a folder that's a Docker volume (a shared volume or a host-mounted volume that persists beyond the container's lifetime) or in an external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span> <span data-ttu-id="d8309-125">外部プロバイダーは、アプリがネットワークの共有ボリュームにアクセスできない場合にも web ファームのシナリオで役に立ちます (を参照してください[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)詳細については)。</span><span class="sxs-lookup"><span data-stu-id="d8309-125">An external provider is also useful in web farm scenarios if apps can't access a shared network volume (see [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) for more information).</span></span>

> [!WARNING]
> <span data-ttu-id="d8309-126">開発者が上記で説明したルールの上書き、特定のキー リポジトリにデータ保護システムを指す場合は、残りの部分でのキーの自動暗号化が無効です。</span><span class="sxs-lookup"><span data-stu-id="d8309-126">If the developer overrides the rules outlined above and points the Data Protection system at a specific key repository, automatic encryption of keys at rest is disabled.</span></span> <span data-ttu-id="d8309-127">残りの部分にある保護を使用して再度有効にすることができます[構成](xref:security/data-protection/configuration/overview)です。</span><span class="sxs-lookup"><span data-stu-id="d8309-127">At-rest protection can be re-enabled via [configuration](xref:security/data-protection/configuration/overview).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="d8309-128">キーの有効期間</span><span class="sxs-lookup"><span data-stu-id="d8309-128">Key lifetime</span></span>

<span data-ttu-id="d8309-129">キーでは、既定では 90 日間有効期間があります。</span><span class="sxs-lookup"><span data-stu-id="d8309-129">Keys have a 90-day lifetime by default.</span></span> <span data-ttu-id="d8309-130">キーが期限切れになったときに、アプリは自動的に新しいキーを生成し、アクティブ キーとして新しいキーを設定します。</span><span class="sxs-lookup"><span data-stu-id="d8309-130">When a key expires, the app automatically generates a new key and sets the new key as the active key.</span></span> <span data-ttu-id="d8309-131">提供終了になったキーは、システムに残っている限り、アプリは、それらに保護されている任意のデータを暗号化解除できます。</span><span class="sxs-lookup"><span data-stu-id="d8309-131">As long as retired keys remain on the system, your app can decrypt any data protected with them.</span></span> <span data-ttu-id="d8309-132">参照してください[キー管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d8309-132">See [key management](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="d8309-133">既定のアルゴリズム</span><span class="sxs-lookup"><span data-stu-id="d8309-133">Default algorithms</span></span>

<span data-ttu-id="d8309-134">使用される既定のペイロード保護アルゴリズムは AES 256-CBC 機密性と HMACSHA256 の信頼性です。</span><span class="sxs-lookup"><span data-stu-id="d8309-134">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="d8309-135">512 ビット マスター _ キー、90 日ごとに変更はペイロードあたりごとにこれらのアルゴリズムに使用される 2 つのサブ キーを派生させるために使用します。</span><span class="sxs-lookup"><span data-stu-id="d8309-135">A 512-bit master key, changed every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="d8309-136">参照してください[サブキーを派生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d8309-136">See [subkey derivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) for more information.</span></span>

## <a name="see-also"></a><span data-ttu-id="d8309-137">関連項目</span><span class="sxs-lookup"><span data-stu-id="d8309-137">See also</span></span>

* [<span data-ttu-id="d8309-138">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="d8309-138">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)