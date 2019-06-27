---
title: "Nodejs Tofixed"
date: 2018-03-02T15:05:07+08:00
draft: false
tags: ["nodejs"]
categories: ["nodejs"]
subtitle: ""
descriptions: ""
bigimg:
---

nodejs系列之toFixed

在遇到，4147.8015这样的浮点数时的，toFixed()的替代方案。

通过下面这一段，应该知道，这里的toFixed2 中的 “9”（是可以设置成限定的位数的准确度的，不应该设置成“2”，“2”太小了，是有问题的，所以设置成大数“9”）。

    tom@adata:~/m6s/nodejs/module/toFixed$ node
    > var aux = Math.pow(0.1, 1 + 2)
    undefined
    > var number = 14.44999
    undefined
    > number + aux
    14.45099
    > (number + aux).toFixed(1)
    '14.5'

下面，直接上代码吧。共2个js文件。写法不同，意思相同。

toFixedTest1.js

    module.exports.MathHelper = MathHelper;

    var MathHelper = (function() {
        this.round = function(number, numberOfDecimals) {
            var aux = Math.pow(10, numberOfDecimals);
            return Math.round(number * aux) / aux;
        };
        this.floor = function(number, numberOfDecimals) {
            var aux = Math.pow(10, numberOfDecimals);
            return Math.floor(number * aux) / aux;
        };
        this.ceil = function(number, numberOfDecimals) {
            var aux = Math.pow(10, numberOfDecimals);
            return Math.ceil(number * aux) / aux;
        };
        this.toFixed = function(number, numberOfDecimals) {
            return (number + 0.000001).toFixed(numberOfDecimals);
        };
        this.toFixed2 = function(number, numberOfDecimals) {
            var aux = Math.pow(0.1, numberOfDecimals + 9); // 这里的这个9,只是让它小一点，再小一点。
            return (number + aux).toFixed(numberOfDecimals);
        };
        this.toFixed3 = function(number, numberOfDecimals) {
            var aux = Math.pow(0.1, numberOfDecimals);
            console.log("aux =>", aux);
            return (number + aux).toFixed(numberOfDecimals);
        };
        return {
            round: round,
            floor: floor,
            ceil: ceil,
            toFixed: toFixed,
            toFixed2: toFixed2,
            toFixed3: round
        };
    })();

    var ToFixedHelper = (function() {
        this.toFixed = function(number, numberOfDecimals) {
            var aux = Math.pow(0.1, numberOfDecimals);
            return (number + aux).toFixed(numberOfDecimals);
        };
        return {
            toFixed: toFixed
        }
    })();

    console.log(MathHelper.round(5.175, 2));
    console.log(MathHelper.toFixed(5.175, 2));
    console.log(MathHelper.toFixed(4147.8015, 3));
    console.log(MathHelper.toFixed2(4147.8015, 3));
    console.log(ToFixedHelper.toFixed(4147.8015, 3));

    var number = 4147.8015;
    var num = 10000;
    console.time("toFixed");
    for (var i = 0; i < num; i++) {
        number.toFixed(3);
    }
    console.timeEnd("toFixed");

    console.time("toFixed1");
    for (var i = 0; i < num; i++) {
        MathHelper.toFixed(number, 3);
    }
    console.timeEnd("toFixed1");

    console.time("toFixed2");
    for (var i = 0; i < num; i++) {
        MathHelper.toFixed2(number, 3);
    }
    console.timeEnd("toFixed2");


toFixedTest2.js

    var MathHelper = {
        toFixed : function(number, numberOfDecimals) {
            // var aux = Math.pow(0.1, numberOfDecimals + 2) + Math.pow(0.1, numberOfDecimals + 1);
            var aux = 0.000001; // Math.pow(0.1, numberOfDecimals + 2);
            return (number + aux).toFixed(numberOfDecimals);
        },
        toFixed2 : function(number, numberOfDecimals) {
            // var aux = Math.pow(0.1, numberOfDecimals + 2) + Math.pow(0.1, numberOfDecimals + 1);
            var aux = Math.pow(0.1, numberOfDecimals + 9);
            return (number + aux).toFixed(numberOfDecimals);
        }
    };

    console.log(MathHelper.toFixed(5.175, 2));
    console.log(5.175.toFixed(2));
    console.log(MathHelper.toFixed(4147.8015, 3));
    console.log(4147.8015.toFixed(3));
    console.log(MathHelper.toFixed(14.499, 1));
    console.log(14.499.toFixed(1));

    console.log(14.44999.toFixed(1));
    console.log(MathHelper.toFixed(14.44999, 1));
    console.log(MathHelper.toFixed2(14.44999, 1));

    var number = 4147.8015;
    var num = 10000;
    console.time("toFixed");
    for (var i = 0; i < num; i++) {
        number.toFixed(3);
    }
    console.timeEnd("toFixed");

    console.time("toFixed1");
    for (var i = 0; i < num; i++) {
        MathHelper.toFixed(number, 3);
    }
    console.timeEnd("toFixed1");

    console.time("toFixed1");
    for (var i = 0; i < num; i++) {
        MathHelper.toFixed(number, 3);
    }
    console.timeEnd("toFixed1");
