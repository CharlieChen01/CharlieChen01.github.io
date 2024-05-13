+++
title = 'The Solidity Indexed Modifier'
date = 2024-04-26T16:16:37+08:00
tags = ['Modifier', 'Solidity']
draft = false
+++

In Solidity, the indexed modifier is used to declare parameters in events (event) and indicates that the value of the parameter should be indexed. The purpose of the indexed modifier is to create a searchable index for event parameters, allowing for more efficient filtering and retrieval of events. When a parameter is declared as indexed, the Solidity compiler creates an index for that parameter in the event log.

Here are some key points about the indexed attribute in Solidity events:

## Using indexed in Event Definitions:

You can add the indexed attribute to parameters in your event definitions. Up to three parameters can be indexed. For example:
event Transfer(address indexed \_from, address indexed \_to, uint indexed \_amount);
In the example above, both \_from and \_to parameters are set as indexed, meaning we can listen for these events and filter by these two parameters’ values using Web3.js or Ethers.js.

## Filtering Events:

With indexed parameters, we can filter events more precisely. Here are some examples:

- Filtering events where a certain address is the sender:

```JavaScript

let filter = this.contract_instance.filters.Transfer(this.active_wallet.address, null, null);
this.contract_instance.on(filter, (from, to, value, event) => {
    console.log("Listening to Ether transfer events:");
    console.log(`from: ${from} to: ${to} value: ${value}`);
});
```

- Filtering events where a certain address is the receiver:

```JavaScript

let filter = this.contract_instance.filters.Transfer(null, this.active_wallet.address, null);
this.contract_instance.on(filter, (from, to, value, event) => {
    console.log("Listening to Ether transfer events:");
    console.log(`from: ${from} to: ${to} value: ${value}`);
});
```

- Filtering events with a specific amount (note the value is in hexadecimal):

```JavaScript

// Filter for events with value of 100
let filter = this.contract_instance.filters.Transfer(null, null, "0x64");

// Filter for events with values in an array
let filter = this.contract_instance.filters.Transfer(null, null, ["0x63", "0x64", "0x65"]);

this.contract_instance.on(filter, (from, to, value, event) => {
    console.log("Listening to Ether transfer events:");
    console.log(`from: ${from} to: ${to} value: ${value}`);
});
```

In summary, the indexed attribute allows us to handle event parameters in Solidity more effectively, enhancing the flexibility and usability of events.

在 Solidity 中，indexed 修饰符用于声明事件（event）中的参数，并指示将该参数的值进行索引。indexed 修饰符的作用是为事件参数创建一个可搜索的索引，以便更高效地过滤和检索事件。当一个参数被声明为 indexed 时，Solidity 编译器会在事件的日志中为该参数创建一个索引。

具体来说，indexed 属性在 Solidity 事件中非常重要，因为它允许我们在事件参数上进行值过滤。以下是一些关于 indexed 属性的重要信息：

## 定义事件时使用 indexed 属性：

你可以在事件定义中的参数上增加 indexed 属性。最多可以对三个参数增加这样的属性。例如：
event Transfer(address indexed \_from, address indexed \_to, uint indexed amount);
在上面的示例中，\_from 和 \_to 参数都被设置为 indexed，这意味着我们可以在 Web3.js 或 Ethers.js 中通过对这两个参数进行值过滤来监听相应的事件。

## 过滤事件：

通过使用 indexed 参数，我们可以更精确地过滤事件。以下是一些示例：

- 过滤某个地址为发送者的事件：

```JavaScript

let filter = this.contract_instance.filters.Transfer(this.active_wallet.address, null, null);
this.contract_instance.on(filter, (from, to, value, event) => {
    console.log("监听发送以太坊事件:");
    console.log(`from: ${from} + to: ${to} + value: ${value}`);
});
```

- 过滤某个地址为接收者的事件：

```JavaScript

let filter = this.contract_instance.filters.Transfer(null, this.active_wallet.address, null);
this.contract_instance.on(filter, (from, to, value, event) => {
    console.log("监听发送以太坊事件:");
    console.log(`from: ${from} + to: ${to} + value: ${value}`);
});
```

- 过滤指定数量的事件（注意参数值是十六进制）：

```JavaScript

// 过滤 value 为 100 的事件
let filter = this.contract_instance.filters.Transfer(null, null, "0x100");

// 过滤 value 为数组中值的事件
let filter = this.contract_instance.filters.Transfer(null, null, ["0x99", "0x100", "0x101"]);

this.contract_instance.on(filter, (from, to, value, event) => {
    console.log("监听发送以太坊事件:");
    console.log(`from: ${from} + to: ${to} + value: ${value}`);
});
```

总之，indexed 属性使我们能够更有效地处理 Solidity 中的事件参数，从而提高了事件的灵活性和可用性。
