---

copyright:

  years: 2018
lastupdated: "2019-01-31"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 更新およびスケーリング
{: #update-scale}

{{site.data.keyword.cfee_full_notm}} サービス・インスタンスを最新バージョンに更新すると、最新の CFEE 機能およびフィックスを利用できます。 CFEE の更新には、Cloud Foundry と CFEE 補助サービス (Kubernetes、Cloud Object Storage、または Compose for PostgreSQL) の新規バージョンが含まれていることがあります。  ただし、必ずしもすべての CFEE 更新に Cloud Foundry と CFEE 補助サービスの新規バージョンが含まれているわけではありません。

CFEE バージョンの更新プログラムは、CFEE コンポーネントを含むコントロール・プレーンに対してと、セルに対して実行します。 また、アプリケーション・セルを追加または削除して、CFEE インスタンスの容量をスケーリングすることもできます。

**注:** バージョンの更新中やセルの追加/削除中には、_「概要」_、_「リソース使用量」_、および_「更新およびスケーリング (Update and Scaling)」_の各ページに表示される一部のメトリック (メモリーや CPU など) が使用不可になる場合があります。

## CFEE バージョンの更新
{: #update}

CFEE インスタンスを新規バージョンに更新するには、ユーザーに以下の権限が必要です。
   * CFEE インスタンスに対する_エディター_ 以上の役割。
   * CFEE がプロビジョンされている Kubernetes クラスターに対する_オペレーター_ 以上の役割。

CFEE インスタンスの CFEE バージョンの更新は、2 つのステップからなるプロセスです。 まず、CFEE のコンポーネントをホストするコントロール・プレーンを更新します。 次に、アプリケーションをホストするセルを更新します。

CFEE を新規バージョンに更新する際には、以下の規則と制約が適用されます。
* まずコントロール・プレーンを更新する必要があります。 コントロール・プレーンが更新されると、セルを更新できるようになります。
* アプリケーション・セルはコントロール・プレーンのバージョンまでしか更新できません。  つまり、コントロール・プレーンの CFEE バージョン・レベルをアプリケーション・セルより高くすることはできますが、その逆はできません。

CFEE インスタンスの CFEE バージョンを更新するには、以下のようにします。
1. [{{site.data.keyword.Bluemix_notm}} ダッシュボード](https://cloud.ibm.com/dashboard/apps/)に移動し、更新する {{site.data.keyword.cfee_full_notm}} を開きます。
2. ナビゲーション・ペインの_「操作」_項目の下にある**「更新とスケーリング」**ページに移動します。
3. (オプション)_「コントロール・プレーン」_表で該当行を展開すると、コントロール・プレーンのワーカー・ノードが表示されます。
4. **「更新」**をクリックします。
5. _「コントロール・プレーンの更新 (Update Control Plane)」_ダイアログで、使用可能な CFEE バージョンの 1 つを選択し、**「更新」**をクリックします。更新には約 45 分かかります。  バージョンの説明に、選択した CFEE バージョンのパッケージに含まれているコンポーネントのバージョンと、そのバージョンで提供される内容についての_「新機能」_文書へのリンクが表示されます。
6. _「ノード状況 (Nodes Status)」_列に更新の進行状況が表示されます。 更新が完了すると、_「バージョン」_列に新しい CFEE バージョンが表されます。
7. コントロール・プレーン・セルの更新が完了したら、_「セル (Cells)」_表を見つけて**「更新」**をクリックします。
8. _「コントロール・プレーンの更新 (Update Control Plane)」_ダイアログで、CFEE バージョンを選択し、*「更新」*をクリックします。 セルはコントロール・プレーンのバージョンまでしか更新できないので、セルの更新可能なバージョンだけが表示されます。 単一の更新操作ですべてのセルを更新できます。
9. 複数のセルが順次更新されていきます。 _「ノード状況 (Nodes Status)」_列に、各セルの更新の進行状況が表示されます。 セルが更新されるにつれ、セルの新しいバージョンが_「バージョン」_列に反映されます。

## バージョン更新中の中断
{: #update-disruption}

Cloud Foundry コントロール・プレーンの更新では中断は発生しません。  一方、Cloud Foundry セルを新しい CFEE バージョンに更新する際には、CFEE 環境の稼働が中断する可能性があります。  セルは一度に 1 つずつ更新されるので、あるセルで実行されていたアプリケーション・インスタンスが更新完了後に稼働状態に戻ると、他のセルが更新されて停止状態になります。 ただし、状況によっては、CFEE で実行される一部のアプリケーションおよびサービスが使用不可になることがあります。 例えば、CFEE に Cloud Foundry セルが 2 つあり、容量がフルに使用されている場合は、バージョン更新でインスタンスが 1 つだけになるとアプリケーションが停止します。セルの更新中にアプリケーション・インスタンスを他方のセルに移動できないので、結果的にアプリケーションが使用不可になるためです。  アプリケーションごとに 3 つのセルと 3 つのインスタンスがある場合でも、バージョンの更新中にアプリケーションが一時的に使用できなくなることがあります。

バージョン更新の際に、CFEE の_「概要」_ページに表示されるセル・ノード・ゲージのメモリーおよび CPU のメトリック (Kubernetes クラスターが生成するデータ) と、Grafana の_「ノード」_ダッシュボードに表示される同じメトリック (オペレーティング・システム・レベルで生成されるデータ) が一致しないことがあります。

更新が進行している間は、CFEE コンポーネントの状況が「ヘルス・チェック」ページに表示されます。  再始動には約 45 分かかります。

## バージョン・サイクルおよびサポート・ポリシー
{: #version-cycle}

通常、Cloud Foundry Enterprise Environment サービスは、新しいバージョンを定期的にリリースします。 サービスのバージョンは、セマンティックなバージョン管理形式 _**Major.Minor.Patch**_ (https://semver.org/) に従います。

_マイナー_・バージョンは定期的に (通常は毎月) 増分されます。 _メジャー_・バージョンも増分されますが、その頻度は低くなります (通常は大きな変更が行われた場合に増分されます)。  _メジャー_・バージョンに更新する際には、アップグレード中に中断が発生する可能性があります。 _パッチ_はフィックスを提供するものであり、入手可能な最新バージョンに適用されます。 

システムのダウン時間を防ぐために、CFEE インスタンスは、現行バージョンより 2 つ上の _メジャー_・バージョンまでしか更新できません。 前の例を使用すると、CFEE インスタンスの現行バージョンが 3.1.2 でターゲット・バージョンが 7.0.0 の場合は、CFEE インスタンスをまずバージョン 5.x.x に更新してから、次にターゲット・バージョンの 7.0.0 に更新する必要があります。

定期的に提供される新機能および既存機能の拡張は、最新バージョンでのみ使用できます。 最新リリースの重要なフィックスは、最新の 3 つの_メジャー_・バージョン (最新バージョンとそれより前の 2 つのバージョン) でのみ提供されます。 例えば、バージョン 4.x.x、3.x.x、および 2.x.x が提供されている場合、バージョン 1.x.x はサポートされません (つまり、バージョン 1.x.x のフィックスのパッチは提供されません)。  

パッチは、特定の_メジャー_ のうち使用可能な最新バージョンに対して提供されます。 例えば、CFEE インスタンスがバージョン 3.1.2 を実行していて、3.x.x _メジャー_ の使用可能な最新バージョンがバージョン 3.3.4 である場合、パッチはバージョン 3.3.x のコード・ベースに対して提供されます。つまり、パッチは (新規の) バージョン 3.3.5 で提供されることになります。 バージョン 3.1.2 の CFEE インスタンスがこの新しいパッチを受け取るためには、バージョン 3.3.5 に更新する必要があります (バージョン 3.3.4 に更新しただけでは、バージョン 3.3.5 でのみ提供されるパッチで修正される問題を解決できないからです)。

## CFEE インフラストラクチャーのスケーリング
{: #scale}

CFEE インスタンスの Cloud Foundry セルを追加または削除するには、以下の権限が必要です。
* CFEE インスタンスに対する_管理者_ 以上の役割。
* CFEE がプロビジョンされている Kubernetes クラスターに対する_オペレーター_ 以上の役割。

CFEE インスタンスにアプリケーション・セルを追加するには、以下のようにします。
1. [{{site.data.keyword.Bluemix_notm}} ダッシュボード](https://cloud.ibm.com/dashboard/apps/)に移動し、セルを追加する {{site.data.keyword.cfee_full_notm}} を開きます。
2. _「セル」_表の近くにある**「Cloud Foundry セル数の変更」**をクリックして、_「Cloud Foundry セルの追加」_ページを開きます。
3. _「Cloud Foundry セル数の変更」_ページで、「ターゲット・セル数」フィールドのセルの総数を選択します。このページには、セルが追加される CFEE インスタンスの地域と場所も表示されます。また、_「現行セル数」_フィールドにはセルの現在の数が、「ノード・サイズ」フィールドには追加するセルの容量 (現在のセルの容量と等しい) も表示されます。ページの右側のペインには、追加されたセルの見積もりコストと、環境の新しい合計見積コストが表示されます。
4. **「セルの追加」**をクリックします。_「セルの追加」_ダイアログで、**「追加」**をクリックします。
5. _「更新とスケーリング」_ページに戻ります。新しいセル・ノードごとに、新しい行が_「セル・ノード」_表に追加されます。「ノードの状況」列に、CFEE 環境へのセルの追加およびデプロイの進行状況が示されます。
