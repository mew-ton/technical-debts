---
type: lesson
title: ""
template: welcome
---

## 見た目の共通化

---

### 概要

見た目が同じだからといって、異なる**ドメイン**や**意味合い**を持つ要素を共通のコンポーネントとして切り出してしまうことで発生する技術負債です。

この状態では、後から追加される要件や振る舞いの違いにより、コンポーネント内のロジックが複雑化し、**メンテナンス性の低下**を引き起こします。

---

### なぜ発生するのか？

- **見た目の類似性**が「同じもの」として誤認される
- 要件や振る舞いの違いを考慮せずに、安易に共通化してしまう
- 再利用を優先しすぎた結果、責務が曖昧になる

---

### 発生する問題

1. **責務の混在**
   異なるドメインや目的を持つ要素を1つのコンポーネントに押し込めることで、責務が曖昧になります。

2. **コンポーネントの肥大化**
   異なる要件が追加されるたびに条件分岐が増え、コンポーネントが複雑化します。

3. **メンテナンス性の低下**
   ある変更が別のドメインの挙動に影響を与える可能性があり、修正の難易度が高まります。

4. **変更コストの増加**
   見た目が同じであっても、役割ごとに異なる挙動を追加するため、再利用の恩恵が薄れます。

---

### 回避するためのアプローチ

1. **ドメインごとに分離する**
   見た目が似ていても、異なる目的や意味を持つ要素は別々のコンポーネントとして管理します。

2. **責務を明確化する**
   それぞれのコンポーネントが持つ役割や責務を明示し、責務に応じた設計を行います。

3. **再利用の基準を見直す**
   「見た目の同一性」, 「再利用しやすさ」よりも「責務の明確さ」を優先し、過剰な共通化を避けます。

4. **意図的な共通部分のみを抽出**
   本当に共通する要素（スタイルや小さなパーツ）だけを別コンポーネント化し、責務ごとのコンポーネントで利用するようにします。
