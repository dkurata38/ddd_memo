# 使いながら覚えるSpring Data JDBC

## 0. はじめに

### 概要

[Spring Data JDBCの使い方メモ](https://qiita.com/dkurata38/items/33e43b6cfc6f2f2bb393)に記載されている内容をチュートリアル形式に整理した記事です。単純なリファレンスとしてSpring Data JDBCの説明をご覧になりたい人は、[Spring Data JDBCの使い方メモ](https://qiita.com/dkurata38/items/33e43b6cfc6f2f2bb393)の記事、もしくはSpring Data JDBCの公式リファレンスをご覧ください。

+ Java 11
+ SpringBoot 2.3.1

### 事前準備

[Spring initializr](https://start.spring.io/)でプロジェクトのセットアップを行います。
この記事では以下の設定値でプロジェクトをセットアップします。

| 設定項目 | 値 |
| --- | --- |
| Project | Maven |
| Language | Java |
| SpringBoot | 2.3.1 |
| Project Metadate -group- | com.example |
| Project Metadate -artifact- | demo |
| Project Metadate -name- | demo |
| Project Metadate -package name- | com.example.demo |
| Project Metadate -packaging- | jar |
| Project Metadate -java- | 11 |
| Dependencies | Lombok, Spring Data JDBC, H2 Database, PostgreSQL Driver |

## 1. 単一テーブルへの操作を行う

### 目標


User


### サンプルコード

### 解説

## 2. テストをする

基本のRepositoryの定義パターン		
エンティティの定義パターン

## 3. 単一テーブルに対してプライマリキー以外の条件を指定して検索をする

### 解説

メソッドを追加すればできる
メソッドの命名規則

## 4. 複数テーブルへの操作を行う

### 解説

基本のパターン（単体とSet）
応用パターン（Mapなど）

## 5. 複数のカラムを一つのフィールドにマッピングする

## 6. IDをアプリケーション側で発行する

## 7. フィールドにユーザー定義の型の変数を定義する