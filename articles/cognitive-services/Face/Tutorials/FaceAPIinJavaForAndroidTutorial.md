---
title: チュートリアル:Android SDK を使用して画像内の顔を検出し、フレームに収める
titleSuffix: Azure Cognitive Services
description: このチュートリアルでは、Face API を使用して画像内の顔を検出し、フレームに収めるシンプルな Android アプリを作成します。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 11/12/2018
ms.author: pafarley
ms.openlocfilehash: 18e7323f16970b5ab365688b2af896b2c90b9099
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858707"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>チュートリアル:画像の中にある顔を検出してフレームに収める Android アプリの作成

このチュートリアルでは、Java SDK を介して Azure Face API を使用して画像内の顔を検出するシンプルな Android アプリケーションを作成します。 このアプリケーションでは、選択した画像が表示され、検出されたそれぞれの顔の周囲にフレームが描画されます。

このチュートリアルでは、次の操作方法について説明します。

> [!div class="checklist"]
> - Android アプリケーションを作成する
> - Face API クライアント ライブラリをインストールする
> - クライアント ライブラリを使用して画像の中にある顔を検出する
> - 検出された顔の周囲にそれぞれフレームを描画する

![Android のスクリーンショット (赤色の四角形のフレームに顔を収めた写真)](../Images/android_getstarted2.1.PNG)

完全なサンプル コードは、GitHub 上の [Cognitive Services Face Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) リポジトリにあります。

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/) を作成してください。 

## <a name="prerequisites"></a>前提条件

- Face API サブスクリプション キー。 無料試用版のサブスクリプション キーは「[Cognitive Services を試す](https://azure.microsoft.com/try/cognitive-services/?api=face-api)」から取得できます。 または、[Cognitive Services アカウントの作成](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)に関するページの手順に従って、Face API サービスをサブスクライブし、キーを取得します。
- [Android Studio](https://developer.android.com/studio/) (API レベル 22 以降) (Face クライアント ライブラリで必要)。

## <a name="create-the-android-studio-project"></a>Android Studio プロジェクトを作成する

新しい Android アプリケーション プロジェクトを作成するには、次の手順に従います。

1. Android Studio で、**[Start a new Android Studio project]\(新しい Android Studio プロジェクトを開始する\)** を選択します。
1. **[Create Android Project]\(Android プロジェクトの作成\)** 画面で、必要に応じて既定のフィールドに変更を加え、**[次へ]** をクリックします。
1. **[Target Android Devices]\(ターゲットの Android デバイス\)** 画面で、ドロップダウン リストから **API 22** 以降を選択し、**[Next]\(次へ\)** をクリックします。
1. **[Empty Activity]\(空のアクティビティ\)** を選択し、**[Next]\(次へ\)** をクリックします。
1. **[Backwards Compatibility]\(下位互換性\)** チェック ボックスをオフにして **[Finish]\(完了\)** をクリックします。

## <a name="add-the-initial-code"></a>初期コードの追加

### <a name="create-the-ui"></a>UI を作成する

*activity_main.xml* を開きます。 Layout Editor で、**[Text]\(テキスト\)** タブを選択し、その内容を次のコードに置き換えます。

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?range=1-18)]

### <a name="create-the-main-class"></a>main クラスを作成する

*MainActivity.java* を開き、既存の `import` ステートメントを次のコードに置き換えます。

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=3-11)]

次に、**MainActivity** クラスの内容を次のコードに置き換えます。 これは、新しいアクティビティを開始するイベント ハンドラーを **Button** 上に作成して、ユーザーが写真を選択できるようにするものです。 写真は **ImageView** に表示されます。

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=29-68)]

### <a name="try-the-app"></a>アプリを試す

**onActivityResult** メソッド内の **detectAndFrame** の呼び出しをコメント アウトします。 次に、メニューの **[Run]\(実行\)** を押してアプリをテストします。 エミュレーターまたは接続されているデバイスのいずれかでアプリが開いたら、下部の **[Browse]** をクリックします。 デバイスのファイルの選択ダイアログ ボックスが表示されます。 画像を選択して、ウィンドウに表示されることを確認します。 その後、アプリを閉じて次の手順に進みます。

![Android スクリーンショット (顔が写った写真)](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Face SDK を追加する

### <a name="add-the-gradle-dependency"></a>Gradle の依存関係を追加する

**[Project]\(プロジェクト\)** ウィンドウで、ドロップダウン リストから **[Android]** を選択します。 **[Gradle Scripts]\(Gradle スクリプト\)** を展開し、*[build.gradle (Module: app)]* を開きます。 次のスクリーンショットに示したように、Face クライアント ライブラリの依存関係 (`com.microsoft.projectoxford:face:1.4.3`) を追加し、**[Sync Now]\(今すぐ同期\)** をクリックします。

![Android Studio スクリーンショット (App build.gradle ファイル)](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Face 関連のプロジェクト コードを追加する

**MainActivity.java** に戻り、次の `import` ステートメントを追加します。

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=13-14)]

次に、**MainActivity** クラス内の **onCreate** メソッドの上に次のコードを挿入します。

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=17-27)]

`<Subscription Key>` を実際のサブスクリプション キーに置き換える必要があります。 さらに、キーの適切なリージョン識別子を使用して、`<API endpoint>` を実際の Face API エンドポイントに置き換えます (全リージョンのエンドポイント一覧については、[Face API のドキュメント](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)を参照)。 無料試用版のサブスクリプション キーは、**westus** リージョンで生成されます。

**[Project]\(プロジェクト\)** ウィンドウで **[app]\(アプリ\)**、**[manifests]\(マニフェスト\)** を順に展開し、*AndroidManifest.xml* を開きます。 `manifest` 要素の直接の子として、次の要素を挿入します。

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?range=5)]

## <a name="upload-image-and-detect-faces"></a>画像をアップロードして顔を検出する

このアプリでは、**FaceServiceClient.detect** メソッドを呼び出して顔を検出します。このメソッドは、[Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) REST API をラップし、**Face** インスタンスのリストを返します。

返される **Face** にはそれぞれ、その場所を示す四角形と一連の任意の顔属性が含まれます。 この例では、顔の四角形のみが要求されています。

次の 2 つのメソッドを **MainActivity** クラスに挿入します。 顔検出が完了すると、アプリは **drawFaceRectanglesOnBitmap** メソッドを呼び出して **ImageView** を変更することに注意してください。 次に、このメソッドを定義します。

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=70-150)]

## <a name="draw-face-rectangles"></a>顔の四角形を描画する

次のヘルパー メソッドを **MainActivity** クラスに挿入します。 このメソッドでは、各 **Face** インスタンスの四角形の座標を使用して、検出されたそれぞれの顔を囲む四角形を描画します。

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=152-173)]

最後に、**onActivityResult** 内の **detectAndFrame** メソッドの呼び出しをコメント解除します。

## <a name="run-the-app"></a>アプリの実行

アプリケーションを実行し、顔が写っている画像を参照します。 Face サービスが応答するまで数秒お待ちください。 画像内のそれぞれの顔に赤色の四角形が表示されます。

![顔を囲む赤い四角形が描かれた Android のスクリーンショット](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、Face API Java SDK を使用するための基本的なプロセスを学習し、画像内の顔を検出してフレームに収めるアプリケーションを作成しました。 次は、顔検出の詳細について学習してください。

> [!div class="nextstepaction"]
> [画像内の顔を検出する方法](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
