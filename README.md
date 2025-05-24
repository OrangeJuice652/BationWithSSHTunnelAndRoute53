# Bastion Server with Reverse SSH Tunnel on AWS

このテンプレートは、AWS上にBastion（踏み台）サーバーを構築し、**自宅PC（ホームサーバー）へ外部からSSH接続する環境を整備**します。  
Route 53 でドメイン名による接続も可能とし、リバースSSHトンネルによって固定IPなしでも接続が維持できます。

---

## CloudFormation パラメータ一覧

| パラメータ名 | 説明 |
|--------------|------|
| `SSHPublicKey` | Bastion に登録する SSH 公開鍵（NoEcho により CLI 出力されません） |
| `SSHHomeServerUserName` | 自宅PC（ホームサーバー）のログインユーザー名 |
| `SSHHomeServerPrivateKey` | 自宅PC に接続するための秘密鍵名（例: `id_rsa_home`） |
| `HostedZoneName` | Route 53 ホストゾーン名（例: `example.com.`） |
| `RecordName` | 登録するFQDN（例: `bastion.example.com.`） |
| `InstanceType` | Bastion 用 EC2 のインスタンスタイプ（例: `t3.micro`） |
| `ImageId` | 使用する AMI ID（例: `ami-0abcdef1234567890`） |

---

## アーキテクチャ構成

``` mermaid
graph TD
  C[接続元PC（例：ノートPC）] -->|ssh bastion.example.com| B[Bastionサーバー（AWS）]
  A[自宅PC（ホームサーバー）] -->|ssh -R 2222:localhost:22 bation.example.com| B
```

---

## 注意

- 踏み台->ホームサーバー接続のための秘密鍵は、事前にホームサーバーから踏み台に設置しておくこと
