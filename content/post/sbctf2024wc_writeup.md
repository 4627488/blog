---
title: "SBCTF wintercamp2024 部分wp"
description: 
date: 2024-02-02T19:00:00Z
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

## Week2 - Misc - ez_brainfuzz

### Description

和靶机交互，发送一段Brainfuck代码，靶机会返回输出和flag的md5的相似度.
如果完全一致就会给出flag

### Solution

暴力枚举即可，注意到md5是32位的hex，直接枚举每一位即可，枚举第 $i$ 位时把其他位用`'z'`补齐即可，这样如果这一位是正确的，给出的相似度应该大于0，反之则没有任何位相同，应该得到0%。

对于要输出的字符串，生成Brainfxxk代码

```python
def generate_brainfuck_code(input_string):
    brainfuck_code = ""
    for char in input_string:
        ascii_value = ord(char)
        brainfuck_code += "+" * ascii_value + "."
        brainfuck_code += ">"
    return brainfuck_code
```

枚举

```python
def get_predict(c, index):
    st = ""
    for ind in range(32):
        if ind == index:
            st += c
        else:
            st += "z"
    return st

hex_list = ['0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F']

host=''
port=

if __name__ == "__main__":
    conn = remote(host=host,port=port)
    print(conn.recv().decode())
    ans = ""
    for pos in range(32):
        this_ans = ""
        for pred in hex_list:
            check = get_predict(pred, pos)
            print(check)
            check = generate_brainfuck_code(check)
            conn.send(check + "\n")
            ret = conn.recv().decode()
            print(ret)
            sm = float(ret.split("%")[0].split(":")[1])
            if sm > 0:
                this_ans = pred
                break
        ans += this_ans
        print(ans)
```

## Week2 - Misc - qrazy_pic_encode

### Description

解密二维码图片，加密方式是：

若源图片中这个像素为黑色：加密后变为20个随机数经过离散余弦变换（Discrete Cosine Transform）并去除第0位得到的19个值。

若源图片中这个像素为白色：加密后变为20个随机数经过**两次**离散余弦变换（Discrete Cosine Transform）并去除第0位得到的19个值。

### Solution

注意到是二分类问题，直接逻辑回归即可

```python
import numpy as np
import ast
from PIL import Image
import random
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import classification_report
from scipy.fftpack import dct

datas=[]
labels=[]

def normalize(lst):
    min_value = min(lst)
    max_value = max(lst)
    normalized_values = [(x - min_value) / (max_value - min_value) for x in lst]
    return normalized_values

#generate dataset/label
for i in range(50000):
    y = [random.random() for _ in range(20)]
    y1 = dct(y)
    if i%2:
        y1 = dct(y1)
    datas.append(normalize(y1[1:]))
    labels.append(i%2)
X_train, X_test, y_train, y_test = train_test_split(
    datas, labels, test_size=0.2, random_state=42
)
print('data generated')

#train model on CPU
model = LogisticRegression()
model.fit(X_train, y_train)
print('model trained')

#test model
y_pred = model.predict(X_test)
print(classification_report(y_test, y_pred))

#decode picture
with open("out.txt", "r") as f:
    data = f.read()

data = ast.literal_eval(data)

print(len(data))

img = [model.predict([data[i : i + 19]])[0] for i in range(0, len(data), 19)]
img = np.array(img).reshape((37, 37))
Image.fromarray(img * 255).convert("L").save("decrypted.png")
```
