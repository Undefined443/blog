GPG (GnuPG) 是一种加密工具，用于数据加密和数字签名。

### 密钥配置

```sh
# 生成密钥
gpg --full-generate-key

# 列出密钥
gpg --list-keys  # 列出公钥
gpg --list-secret-keys  # 列出私钥

# 导出密钥
gpg --armor --export [email/ID] > public.asc  # 导出公钥
gpg --export-secret-keys -a [email/ID] > private.asc  # 导出私钥
gpg --dearmor <key_file>  # 将文本密钥转换为二进制密钥

# 导入密钥
gpg --import [key_file]  # 导入密钥
gpg --sign-key [email/ID]  # 为导入的密钥签名（信任）

# 删除密钥
gpg --delete-key [email/ID]  # 删除公钥
gpg --delete-secret-key [email/ID]  # 删除私钥

# 编辑密钥
gpg --edit-key [email/ID]

# 更改密钥密码
gpg --passwd [email/ID]
```

> 密钥 ID 是密钥指纹的后 8 位或者后 16 位，或者完整的密钥指纹。三者之一都可以用来表示密钥 ID。

### 加密/签名

```sh
# 加密
gpg --recipient [email/ID] --encrypt [input_file]  # 将生成一个 .gpg 文件，包含了加密的文件

# 解密
gpg --decrypt [gpg_file] > [output_file]

# 签名
gpg --sign --local-user [email/ID] [input_file]  # 将生成一个 .gpg 文件，包含了明文文件和文件签名

## 使用分离签名
gpg --detach-sign --local-user [email/ID] [input_file]  # 将生成一个 .sig 二进制文件
gpg --armor --detach-sign --local-user [email/ID] [input_file]  # 将生成一个 .asc 文本文件

# 验证签名
gpg --verify [signature_file] [input_file]  # 可以省略 input_file，此时会以 signature_file 的文件名推测输入文件名
              # 另外，也可以使用 --decrypt 在解密的同时验证签名

# 签名并加密
gpg --sign --recipient [email/ID] --encrypt [input_file]

# 解密并验证签名（和仅解密的命令一样）
gpg --decrypt [gpg_file] > [outputfile]
```

### 使用样例

#### 列出所有公钥

```sh
$ gpg --list-keys
[keyboxd]
---------
pub   ed25519 2023-12-28 [SC]
      564B356F76DEDA922C87D9A6ADA20CA03D7C43B
uid             [ 绝对 ] User Name (Default GPG Key) <example@email.com>
sub   cv25519 2023-12-28 [E]
```

- `pub`: 这行表示一个公钥 (pubkey) 的记录开始。
- `ed25519`: 这是公钥使用的加密算法，ed25519 是一个采用 Edwards-curve Digital Signature Algorithm (EdDSA) 的签名算法，它著名的特点是速度快且安全。这种算法特别适用于创建数字签名。
- `2023-12-28`: 这个日期是公钥的生成日期。
- `[SC]`: 这里的 S 表示签名（Sign），C 表示证书（Certify）。这意味着这个公钥可以用于数字签名和证书其他密钥。
- `564B356F76DEDA922C87D9A6ADA20CA03D7C43B`: 这是公钥的指纹，是识别公钥的一种更精确的方式。它是公钥的一个散列值，可用于在密钥服务器上查找公钥或验证公钥。
- `uid`: 这个字段代表用户身份（User ID），包括名称和电子邮件地址。在这个例子中，表示公钥的拥有者是 “User Name” 且电子邮件地址为 “example@email.com”。
- `[ 绝对 ]`: 这表明信任级别为 “绝对”。在 GPG 中，你可以给一个密钥指定信任级别，以标识你对此密钥所有人身份验证的信任程度。“绝对” 表示你完全信任此密钥。
- `sub`: 这表示一个附属密钥 (subkey)，它关联到上面的主公钥。附属密钥可以用于加密，而主密钥仍然保持签名和证书的任务。
- `cv25519`: 这是附属密钥使用的加密算法，Curve25519 是一种用于创建公钥加密密钥的算法。cv25519 通常用于加密。
- `[E]`: 这表示这个附属密钥用于加密 (Encrypt)。

总的来说，这个输出表示你有一个用 ed25519 算法生成的 GPG 主公钥，它在 2023 年 12 月 28 日生成，并有一个用户身份名为 “User Name” 和对应的电子邮件地址。你还有一个用 cv25519 算法生成的附属密钥用于加密，它也是在同一天生成的。

#### 列出所有私钥

```sh
$ gpg --list-secret-keys
[keyboxd]
---------
sec   ed25519 2023-12-28 [SC]
      564B356F76DEDA922C87D9A6ADA20CA03D7C43B
uid             [ 绝对 ] User Name (Default GPG Key) <example@email.com>
ssb   cv25519 2023-12-28 [E]
```

- `keyboxd` 部分是内部的，通常你不需要关心；它跟 GPG 的密钥存储有关。
- `sec` 表示这是一段私钥（secret key）的信息。ed25519 是私钥使用的加密算法，这里采用的是 Ed25519，一种基于 EdDSA 签名算法的公钥加密算法。Ed25519 被认为是非常安全的，主要用于签名操作。
- `2023-12-28` 表示私钥的生成日期是 2023 年 12 月 28 日。
- `[SC]` 表示这把私钥具有签名（S）和证书（C）的能力。签名用于保护信息和验证作者的身份，而证书能力意味着这把钥匙可以用来为其他的公钥签名，建立信任网络。
- `56EBD36FC71AEDC622DE749A43DA00CC0D17643C` 是私钥的指纹。这是一个独特的标识，用于确切地指代这把私钥。在 GPG 的使用中，指纹用来验证密钥的真实性。
- `uid` 表示用户 ID，这是密钥所有者的身份说明。[ 绝对 ] 表示对该密钥的信任等级是绝对的，即这把密钥完全被信任。User Name (Default GPG Key) <example@email.com> 包含名字、评论（这里用作标识默认的 GPG 密钥）、以及电子邮件地址，这是用来标识密钥所有者的信息。
- `ssb` 是指私钥下的子密钥（secret subkey）。cv25519 指明了子密钥使用的加密算法是 Curve25519，这通常用于加密操作。2023-12-28 是子密钥的生成日期。
- `[E]` 表示子密钥的用途是加密（E）。

#### 加密文件

```sh
gpg --recipient example@email.com --encrypt my_file
```

#### 解密文件

```sh
gpg --decrypt my_file.gpg > my_file
```

#### 对文件签名

```sh
gpg --armor --detach-sign --local-user example@email.com my_file
```

> `--local-user` 及之后的 email/ID 可以省略

#### 验证文件签名

```sh
gpg --verify myfile.asc my_file
```

> `my_file` 可以省略