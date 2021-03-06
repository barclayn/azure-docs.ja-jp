---
title: Azure の更新管理デプロイで事前および事後スクリプトを構成する (プレビュー)
description: この記事では、更新プログラムの展開のための事前および事後スクリプトを構成および管理する方法について説明します。
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4c34c6c6e0a3f618cbd9337993aa6d176962fe6b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428241"
---
# <a name="manage-pre-and-post-scripts-preview"></a>事前および事後スクリプトを管理する (プレビュー)

事前および事後スクリプトを使用すると、更新プログラムの展開の前 (事前タスク) と後 (事後タスク) に Automation アカウントで PowerShell Runbook を実行できます。 事前および事後スクリプトはローカルではなく、Azure コンテキストで実行されます。 事前スクリプトは、更新プログラムの展開の開始時に実行されます。 事後スクリプトは、展開の最後、構成されているすべての再起動の後で実行されます。

## <a name="runbook-requirements"></a>Runbook の要件

Runbook が事前または事後スクリプトとして使用されるようにするには、その Runbook をオートメーション アカウントにインポートして発行する必要があります。 このプロセスの詳細については、「[Runbook の発行](automation-creating-importing-runbook.md#publishing-a-runbook)」を参照してください。

## <a name="using-a-prepost-script"></a>事前および事後スクリプトの使用

更新プログラムの展開で事前または事後スクリプトを使用するには、単純に更新プログラムの展開の作成から始めます。 **[Pre-scripts + Post Scripts (Preview)] (事前スクリプト + 事後スクリプト (プレビュー))** を選択します。 これにより、**[Select Pre-scripts + Post-scripts] (事前スクリプト + 事後スクリプトの選択)** ページが開きます。  

![スクリプトを選択する](./media/pre-post-scripts/select-scripts.png)

使用するスクリプトを選択します。この例では、**UpdateManagement-TurnOnVms** Runbook を使用しました。 Runbook を選択すると、**[Configure Script] (スクリプトの構成)** ページが開きます。パラメーターの値を指定し、**[Pre-Script] (事前スクリプト)** を選択します。 完了したら、**[OK]** をクリックします。

![スクリプトを構成する](./media/pre-post-scripts/configure-script.png)

**UpdateManagement-TurnOffVms** スクリプトについて、このプロセスを繰り返します。 ただし、**[スクリプトの種類]** を選択するときに、**[Post-Script] (事後スクリプト)** を選択します。

**[Selected items] (選択された項目)** セクションに、選択した両方のスクリプトが表示されるようになります。1 つは事前スクリプトであり、もう一方は事後スクリプトです。

![[Selected items] (選択された項目)](./media/pre-post-scripts/selected-items.png)

更新プログラムの展開の構成を完了します。

更新プログラムの展開が完了したら、**[Update deployments] (更新プログラムの展開)** に移動して結果を表示できます。 見てわかるように、事前スクリプトと事後スクリプトの状態が表示されます。

![更新プログラムの結果](./media/pre-post-scripts/update-results.png)

更新プログラムの展開の実行をクリックすると、事前および事後スクリプトの追加の詳細が表示されます。 実行の時点でのスクリプト ソースへのリンクが表示されます。

![展開の実行の結果](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>パラメーターを渡す

事前および事後スクリプトを構成する場合、Runbook のスケジュール設定のように、パラメーターを渡すことができます。 パラメーターは、更新プログラムの展開の作成の時点で定義されます。 標準の Runbook パラメーターに加えて、追加のパラメーターが表示されます。 このパラメーターは **SoftwareUpdateConfigurationRunContext** です。 このパラメーターは JSON 文字列であるため、事前または事後スクリプトで定義すると、このパラメーターは更新プログラムの展開によって自動的に渡されます。 このパラメーターには、更新プログラムの展開に関する情報が含まれています。これは、[SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) によって返される情報のサブセットです。次の表に、この変数で提供されるプロパティを示します。

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext プロパティ

|プロパティ  |説明  |
|---------|---------|
|SoftwareUpdateConfigurationName     | ソフトウェア更新構成の名前        |
|SoftwareUpdateConfigurationRunId     | 実行の一意の ID。        |
|SoftwareUpdateConfigurationSettings     | ソフトウェア更新構成に関連したプロパティのコレクション         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | 更新プログラムの展開の対象となるオペレーティング システム         |
|SoftwareUpdateConfigurationSettings.duration     | ISO8601 に従った `PT[n]H[n]M[n]S` 形式の更新プログラムの展開の実行の最大期間、"メンテナンス期間" とも呼ばれる          |
|SoftwareUpdateConfigurationSettings.Windows     | Windows コンピューターに関連したプロパティのコレクション         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | 更新プログラムの展開から除外される KB の一覧        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | 更新プログラムの展開のために選択された更新プログラムの分類        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | 更新プログラムの展開のための再起動設定        |
|azureVirtualMachines     | 更新プログラムの展開での Azure VM の resourceId の一覧        |
|nonAzureComputerNames|更新プログラムの展開での Azure 以外のコンピューターの FQDN の一覧|

**SoftwareUpdateConfigurationRunContext** パラメーターに渡される JSON 文字列の例を次に示します。

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ], 
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

すべてのプロパティの完全な例は、[ソフトウェア更新構成 (名前で取得)](/rest/api/automation/softwareupdateconfigurations/getbyname#examples) に関するページにあります

## <a name="samples"></a>サンプル

事前および事後スクリプトのサンプルは [Script Center Gallery](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell) にあります。または、Azure Portal 経由でインポートされます。 ポータル経由でインポートするには、Automation アカウントで、**[プロセス オートメーション]** の **[Runbook ギャラリー]** を選択します。 フィルターに **[更新管理]** を使用します。

![ギャラリーの一覧](./media/pre-post-scripts/runbook-gallery.png)

または、次の一覧に示すように、スクリプト名で検索できます。

* 更新管理 - VM 有効
* 更新管理 - VM 無効
* 更新管理 - スクリプトをローカルで実行する
* 更新管理 - 事前および事後スクリプトのテンプレート
* 更新管理 - スクリプトを実行コマンドで実行する

> [!IMPORTANT]
> Runbook をインポートした後、使用できるようにするには、それらを**発行する**必要があります。 それを行うには、Automation アカウントで Runbook を見つけ、**[編集]** を選択して **[発行]** をクリックします。

これらのサンプルはすべて、次の例で定義されている基本的なテンプレートに基づいています。 このテンプレートを使用すると、事前および事後スクリプトで使用する独自の Runbook を作成できます。 Azure に対して認証したり、`SoftwareUpdateConfigurationRunContext` パラメーターを処理したりするための必要なロジックが含まれています。

```powershell
<# 
.SYNOPSIS 
 Barebones script for Update Management Pre/Post 
 
.DESCRIPTION 
  This script is intended to be run as a part of Update Management Pre/Post scripts.  
  It requires a RunAs account. 
 
.PARAMETER SoftwareUpdateConfigurationRunContext 
  This is a system variable which is automatically passed in by Update Management during a deployment. 
#> 
 
param( 
    [string]$SoftwareUpdateConfigurationRunContext 
) 
#region BoilerplateAuthentication 
#This requires a RunAs account 
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection' 
 
Add-AzureRmAccount ` 
    -ServicePrincipal ` 
    -TenantId $ServicePrincipalConnection.TenantId ` 
    -ApplicationId $ServicePrincipalConnection.ApplicationId ` 
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint 
 
$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID 
#endregion BoilerplateAuthentication 
 
#If you wish to use the run context, it must be converted from JSON 
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext 
#Access the properties of the SoftwareUpdateConfigurationRunContext 
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines 
$runId = $context.SoftwareUpdateConfigurationRunId 
 
Write-Output $context 
 
#Example: How to create and write to a variable using the pre-script: 
<# 
#Create variable named after this run so it can be retrieved 
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false 
#Set value of variable  
Set-AutomationVariable –Name $runId -Value $vmIds 
#> 
 
#Example: How to retrieve information from a variable set during the pre-script 
<# 
$variable = Get-AutomationVariable -Name $runId 
#>      
```

## <a name="interacting-with-non-azure-machines"></a>Azure 以外のコンピューターとの対話

事前および事後タスクは Azure コンテキストで実行されるため、Azure 以外のコンピューターにはアクセスできません。 Azure 以外のコンピューターと対話するには、次のものが必要です。

* 実行アカウント
* コンピューターにインストールされた Hybrid Runbook Worker
* ローカルで実行する Runbook
* 親 Runbook

Azure 以外のコンピューターと対話するために、親 Runbook が Azure コンテキストで実行されます。 この Runbook は、[Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) コマンドレットを使用して子 Runbook を呼び出します。 `-RunOn` パラメーターを指定し、スクリプトを実行する Hybrid Runbook Worker の名前を指定する必要があります。

```powershell
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$resourceGroup = "AzureAutomationResourceGroup"
$aaName = "AzureAutomationAccountName"

$output = Start-AzureRmAutomationRunbook -Name "StartService" -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName -RunOn "hybridWorker"

$status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
while ($status.status -ne "Completed")
{ 
    Start-Sleep -Seconds 5
    $status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
}

$summary = Get-AzureRmAutomationJobOutput -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName

if ($summary.Type -eq "Error")
{
    Write-Error -Message $summary.Summary
}
```

## <a name="known-issues"></a>既知の問題

* 事前および事後スクリプトを使用している場合は、パラメーターにオブジェクトまたは配列を渡すことができません。 その Runbook が失敗します。

## <a name="next-steps"></a>次の手順

使用している Windows 仮想マシンの更新プログラムの管理方法を学習するためのチュートリアルに進みます。

> [!div class="nextstepaction"]
> [Azure Windows VM の更新プログラムとパッチの管理](automation-tutorial-update-management.md)

