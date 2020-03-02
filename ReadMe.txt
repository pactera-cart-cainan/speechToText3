使用Azure CLI和ARM部署
登录到 Azure
az login

创建应用程序注册
az ad app create --display-name "speechBotTest001" --password "!QAZ2wsx3edc" --available-to-other-tenants

appId": "9cae2e5d-38a6-43ab-815a-1165fdf08ec1"
password: "!QAZ2wsx3edc"

通过 ARM 模板部署
az deployment create --template-file "template-with-new-rg.json" --location "eastasia" --parameters appId="9cae2e5d-38a6-43ab-815a-1165fdf08ec1" appSecret="!QAZ2wsx3edc" botId="speechBotTest001" botSku=F0 newAppServicePlanName="speechBotTest001Plan" newWebAppName="speechBotTTest001" groupName="speechBotTTest" groupLocation="eastasia" newAppServicePlanLocation="eastasia" --name "speechBotTest001202032"

检索或创建所需的 IIS/Kudu 文件
az bot prepare-deploy --lang Csharp --code-dir "." --proj-file-path "EchoBot1.csproj"

在.csproj目录下创建zip文件（这里另外打开一个PowerShell命令行）
# PowerShell
Compress-Archive -Path * -DestinationPath speechBot.zip

将代码部署到 Azure
az webapp deployment source config-zip --resource-group "speechBotTTest" --name "speechBotTTest001" --src "speechBot.zip"

查看代码部署情报
az webapp deployment source show --resource-group "speechBotTTest" --name "speechBotTTest001"

在speechBotTest001的“机器人通道注册” 面板中，单击“在 Web Chat 中测试”。
测试Web chat OK!

在speechBotTest001的“机器人通道注册” 面板中，单击“信道”，追加直线连接通道“Direct Line”，点击创建完成。在Edit中，取得密钥（web 前端需要）。

在“认知服务”面板中，添加认知服务，在检索栏输入“Speech”，点击创建，名称“speechBotTest”，位置“东亚”，资源组“speechBotTTest”，点击创建完成。

在“所有资源”面板中，点击“speechBotTest”，在“密钥和终结点”取得密钥（web 前端需要）。

在speechBotTest001的“机器人通道注册” 面板中，单击“信道”，追加“Direct Line Speech”，选择资源组“speechBotTTest"下的“speechBotTest”，点击创建完成。

在前端web中确认，speech可以使用。