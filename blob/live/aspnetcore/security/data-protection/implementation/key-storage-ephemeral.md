---
title: "短期のデータ保護プロバイダー"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core 短期データ保護プロバイダーの実装の詳細について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="01ab2-103">短期のデータ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="01ab2-103">Ephemeral data protection providers</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="01ab2-104">アプリケーションが、throwaway が必要なシナリオがある`IDataProtectionProvider`です。</span><span class="sxs-lookup"><span data-stu-id="01ab2-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="01ab2-105">たとえば、開発者が 1 回限りのコンソール アプリケーションで試してみるだけ可能性がありますまたはアプリケーション自体は一時的なものがスクリプト化 (、単体テスト プロジェクト)。</span><span class="sxs-lookup"><span data-stu-id="01ab2-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="01ab2-106">これらのシナリオをサポートするために、 [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)パッケージには、型が含まれています。`EphemeralDataProtectionProvider`です。</span><span class="sxs-lookup"><span data-stu-id="01ab2-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="01ab2-107">この種類の基本的な実装を提供する`IDataProtectionProvider`がキーのリポジトリがメモリ内のみが保持されているし、バッキング ストアに書き込むはありません。</span><span class="sxs-lookup"><span data-stu-id="01ab2-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="01ab2-108">各インスタンス`EphemeralDataProtectionProvider`独自の一意のマスター _ キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="01ab2-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="01ab2-109">したがって場合、`IDataProtector`をルートと、`EphemeralDataProtectionProvider`保護対象のペイロードが生成されますそのペイロードは同等でのみ保護しない`IDataProtector`(指定された同じ[目的](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes)チェーン) ルートと同じ`EphemeralDataProtectionProvider`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="01ab2-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="01ab2-110">次の例では、インスタンス化を示しています、`EphemeralDataProtectionProvider`を保護し、データ保護の解除に使用するとします。</span><span class="sxs-lookup"><span data-stu-id="01ab2-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```