# Flow 快速入门 ｜ 第二周 ｜ 第四天

嗨咯，我是 Jacob 。之前，我们学习了如何修改我们的智能合约、交易、脚本和 dapplib ，以使我们的dApp得以进行。今天，我们将深入研究 DappStarter 上的 UI Harness 和测试，坚持到最后，你将成为 DappStarter 的征服者 :)

让我们开始吧。

# 视频

* [客户端/ UI Harness](https://www.youtube.com/watch?v=-Rka-0ytXhs&feature=youtu.be)
* [测试](https://www.youtube.com/watch?v=0fBhXLJEr9Q)

# 客户端/UI Harness

在本节中，我们将学习如何设置我们的 UI Harness 以及如何将它连接到我们的 DappLib 中。你可以在`packages/client/src/harness/fast_floward-harness`中找到我们的 UI Harness 。让我们再来回顾一下我们这周一直在使用的图表，理解一下DappStarter的整体架构：

![DappStarter Overview](https://github.com/FlowFans/fast-floward-zh/blob/main/week2/day4/images/dappstarter_overview.PNG)

你会看到 UI Harness 可以调用DappLib中设置的 Javascript 函数。让我们来了解一下这是怎样实现的。

## 编写一个功能牌 Action Card

`fast_floward-harness.js`里有一堆我们不需要关心的 Javascript 。我们的主要重点将放在功能牌（Action Card）上，以及如何设置它们。让我们看看我们的UI线束中的一个功能牌的例子，以及它在代码中的样子。

![Picture from UI Harness](https://github.com/FlowFans/fast-floward-zh/blob/main/week2/day4/images/action-card-ui.png)

*这就是功能牌在 UI Harness 中的样子*

![Picture from UI Harness Code](https://github.com/FlowFans/fast-floward-zh/blob/main/week2/day4/images/action-card.png)

*这就是功能牌在 fast_floward-harness.js 中的样子*

一个功能牌元素具有5个 HTML 属性

1) **title** - 这是你的功能牌顶部的文字，代表它的标题。
2) **description** - 这是你的功能牌底部的文字，将对它的作用进行简单描述
3) **action** - 这是 DappLib 中的 Javascript 函数的名称，功能牌将调用这些 JS 函数。
4) **method** - “post"（用于交易）或 "get"（用于脚本）。
5) **fields** - 我们将在 DappLib 中传递给 Javascript 函数的所有字段。每个字段将被映射到一个**部件（widget）**，我将在下一节解释。你将在DappLib中使用`data.{nameOfField}`访问这些字段。

在上面的例子中，你可以看到我们有3个字段，每个字段由一个 widget 表示（将在下面解释）。我们在 DappLib 中调用 kibbleTransferTokens 函数，并将这3个字段传给它。这就是为什么在我们的 DappLib 中，我们写了`data.signer`，`data.recipient`和`data.amount`。

### 部件 Widgets

Widgets 是用来向我们的 DappLib 传递字段的。例如，在上图的功能牌中，你会看到它有3个字段：a signer, an amount, and a recipient.

**部件**通常是**文本部件**或**账户部件**。

#### 账户部件 Account Widgets

帐户部件允许我们选择一个帐户。我们可以用它来选择签字人、转账的受让人等。在我们选择一个账户时，当我们点击 "Submint" 或  "View" 时，签字人或转账的受让人将被传入 DappLib 。

下面是账户部件的一个例子：

![Account Widget](https://github.com/FlowFans/fast-floward-zh/blob/main/week2/day4/images/account_widget.png)

在我的代码中，一个账户部件将包含2个 HTML 属性：

1) **field** - 这是该字段的名称。它必须与我们在**功能牌的 field**属性相匹配。
2) **label** - 这是出现在 UI Harness 上的账户部件旁边的标签。

#### 文本部件 Text Widgest

文本部件允许我们输入数字或字符串。我们可以用它们来输入金额、我们的Kitty item的价格、名称等等。

下面是文本部件的一个例子：

![Text Widget](https://github.com/FlowFans/fast-floward-zh/blob/main/week2/day4/images/text_widget.png)

在我的代码中，一个文本部件将包含3个 HTML 属性：

1) **field** - 这是该字段的名称。它必须与我们在**功能牌的 field**属性相匹配。
2) **label** - 这是出现在UI Harness上的文本部件旁边的标签。
3) **placeholder** - 占位符文本，将出现在UI Harness的文本部件中。

# 测试 Test

如果你已经走到了这一步，你离征服 DappStarter 只差一步了 。漂亮! 

测试文件可以在`/packages/dapplib/test/fast_floward-test.js`中找到。在其中，你会发现一些用于测试的Javascript代码，但你可以忽略其中的大部分。你的重点将是编写 **it()** 函数来运行单个测试。关于设置测试的有几件事你需要知道。

测试是通过使用`yarn test`运行的。就像`yarn start`一样，在根目录运行这个命令。它将启动一个全新的仿真器，这样就不会有来自你的`yarn start`的残余数据。

## 编写一个测试

首先，你将调用你的 DappLib 函数，就像你在 UI Harness 中做的那样。下面是测试的一个例子：

![Test](https://github.com/FlowFans/fast-floward-zh/blob/main/week2/day4/images/test.png)

你会看到，我们通过`DappLib.{nameOfFunction}`调用我们的 DappLib 函数。与 UI Harness 不同的是，我们可以通过 **widgets** 传递数据，我们需要制作一个`testData`对象，其中定义有用于你测试的硬编码值的字段（fields）。

你会注意到，如果我们调用一个发送交易的 DappLib 函数，我们简单地通过`await DappLib.{nameOfFunction}(testData)`来完成。这是因为我们并不关心调用的返回值。然而，如果我们调用一个执行脚本的 DappLib 函数，我们很可能关心其返回值。我们可以这样进行存储：`let res = await DappLib.{nameOfFunction}(testData)`。

之后，你可以用 Javascript 的`assert`来检查你的脚本返回值。脚本的实际值可以通过`res.result`来实现，你可以将其与你期望返回的值进行比较。

# 热加载 Hot Loading

值得注意的是，DappStarter 支持热加载。如果你对你的合约、交易、脚本、dapp-lib.js 或我们将在明天介绍的客户端做了任何改变，你将不必重新运行`yarn start`。你的 dApp 会自动为你重新编译。

# 任务

今天有两个任务，`W2Q6`和`W2Q7`。你**只用**修改`packages/client/src/harness/fast_floward-harness.js`和`packages/dapplib/tests/fast_floward-test.js`。在进行这些任务之前，请确保观看上面的视频。

* `W2Q6` – The Client

在`packages/client/src/harness/fast_floward-harness.js`中，搜索3个上面有 "TODO "的功能牌。你将不得不实现这3个功能牌 auction cards 。我已经为你实现了 DappLib 函数、交易和脚本，所以你所要做的只是功能牌这一步。当制作你的功能牌时，确保你向所有需要使用 **部件 widgets** 的DappLib传入正确的字段（fields）。

请提交已更新的 harness 文件以及你在UI harness上使用的功能牌的图片。

* `W2Q7` – Testing Mania

对于这个任务，在`packages/dapplib/tests/fast_floward-test.js`中，有2个测试供你编写。按照上面关于如何编写测试的指南（文件中的其他测试也是提示），尝试并找出如何写出我为你描述过的测试。这两个测试的每一个上面都有一堆注释，帮助你实现任务。

请务必提交测试文件和你在控制台的测试通过的所有截图。

祝各位一切顺利，DappStarter 冒险者们，我们下次再见~
