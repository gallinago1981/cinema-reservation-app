# このプロジェクトについて
このリポジトリはポートフォリオ用に作成したサンプルプロジェクトです。  
実際の運用を目的としたものではなく、設計・実装スキルを示すためのデモンストレーションとして公開しています。

# Cinema Reservation App
映画館の座席予約システムをオンラインで行えるポートフォリオ用アプリです。

# 技術スタック

## 認証認可
- Amazon Cognito
  - AWSへデプロイするさいには認証認可にAmazon Cognitoを使用することを前提とする
- LocalStack
  - ローカル環境で検証するにあたりAmazon Cognitoの代替としてLocalStackのDockerイメージを使用する

## 決済
- Stripe
  - Test Modeを使用する。


## フロントエンド
- React
- Vite

## BFF
- Node.js
- Fastify

## バックエンド
- Java21
- Spring Boot3.5
- Spring Modulith
- JPA
- Gradle

## インフラ
- Docker
- PostgreSQL
- Redis

## CI/CD
- GitHub Actions

## E2Eテスト
- Playwright
- Testcontainers
- Flyway


# アーキテクチャ

## システム構成

T.B.D PlantUMLで記述した図を埋め込む

## 設計思想

### 疎結合
- フロントエンドとバックエンドはAPIを介して通信することで両者を独立に開発・デプロイを可能とする

### インターフェース設計
- クライアント、サーバー
- フロントエンドとバックエンドとのインターフェース設計にはOpenAPI Specを採用する



### フロントエンド
- BFFパターンを採用
  - 一般ユーザ、管理者向けにそれぞれBFFを分離することでバックエンドサービスへアクセスできる機能を限定する



### バックエンド
- 直接アクセス禁止する
  - バックエンドサービスへアクセス可能とするサービスは必ずBFFを経由する


- モジュラーモノリス
  - バックエンドとしてサービスを細かく分割せずスケールする単位を基準に複数のドメインを同一サービスに集約する
  - モジュラーモノリスを採用することで内部モジュール間はインターフェースを定義し疎結合とする。

# 非機能要件

## 性能要件
- レスポンスタイム
  - 1sec以下
- 同時アクセス
  - T.B.D

## 信頼性要件
- 分散ロックによる座席の二重予約防止
- 仮予約の自動解放
  - 一定時間経過で決済が完了していない座席を解放する
- 決済失敗時の自動ロールバック

## 可用性
- リトライ・タイムアウト設計
- ヘルスチェック

  
# ドメイン設計

- 上映管理 (Screening)
  - 上映作品 (Moview)
  - 公開スケジュール管理 (Schedule)
- 予約管理（Reservation）
- 座席管理（Seating）
- 顧客管理（Customer）
  - 年間有料会員（Member）
  - ゲスト会員（Guest）
- 決済（Payment）
  - クレジットカード決済（Credit Card）

# テスト戦略

## テスト工程
- 単体テスト
  - JUnit5, Mokitoを使用したテスト
  - ドメインロジック、各種モデル変換処理の網羅的なテスト
  - Springコンテキスト全体を起動する @SpringBootTest や @WebMvcTest などは使用しない
- 統合テスト
  - 
- E2Eテスト
  - Playwirghtを利用したシナリオテスト
    - 座席選択から決済完了までのシナリオを実行する
　- 性能テスト 　


# AI / 生成AIの使用について

- 当ポートフォリオを完成させるための壁打ち相手として使用する
  - AIを自信の能力を増幅、拡張させるために使用するが結果を検証できないのを生成するのには使用しない。
- GitHub Copilotによるレビューを行う
  - .copilot-instructions.mdを作成しPRのレビューを実施する
- 単純な実装（プロンプトがシンプル）なものについては生成AIを活用し生産性を上げる
  - 例
    - テーブル定義に則したJPA Entityの作成
    - JPA EntityからDomainモデルへの変換


## License
This project is licensed under custom terms.  
See the [LICENSE](./LICENSE) file for details.
