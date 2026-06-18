# Repository Guidelines

## プロジェクト構成とモジュール配置

このリポジトリは、LogBook 用の uv 管理 Python プロジェクトです。ルートには `pyproject.toml`、`uv.lock`、`.python-version`、`.pre-commit-config.yaml`、`README.md` があります。

設計判断は `docs/adr/` にあります。モジュール境界を変える前に、関連する ADR を確認してください。ADR-0002 では、次の構成を目標としています。

```text
src/logbook/
  main.py
  web/
  api/
  application/
    use_cases/
    ports/
  domain/
  infrastructure/
    db/
  templates/
  static/
tests/
```

業務ルールは `domain/` または `application/` に置きます。HTML/API/DB など外部との接続は `web/`、`api/`、`infrastructure/` に分け、静的ファイルとテンプレートは `static/`、`templates/` に配置します。

## ビルド・テスト・開発コマンド

- `uv sync --group dev`: プロジェクトと開発用依存関係を同期します。
- `uv run pytest`: pytest を quiet モードで実行します。
- `uv run ruff check .`: lint と import 順序を検査します。
- `uv run ruff format --check .`: ファイルを変更せずに format 済みか確認します。
- `uv run ruff format .`: Python ファイルを整形します。
- `uv run ty check`: 静的型チェックを実行します。
- `pre-commit install`: ローカル hook を有効化します。Ruff format の後に Ruff check が走ります。

## コーディングスタイルと命名規則

Python 3.14 構文と 4 スペースインデントを使います。Ruff は行長 100 文字、`py314`、ルール `E`、`F`、`I`、`B`、`UP` で設定されています。

モジュール、関数、変数、テスト名は明確な snake_case、クラス名は PascalCase にします。公開関数や複雑な内部関数には型ヒントを付けてください。`.github/instructions/python.instructions.md` に従い、Python のモジュール、クラス、関数、メソッドには簡潔な Google Style の日本語 docstring を書きます。
ソースコードコメントを遠慮せずに書いてよい。

```python
def sum_numbers(a: int, b: int) -> int:
    """2つの整数を受け取り、その合計を返す。

    Args:
        a (int): 1つ目の整数。
        b (int): 2つ目の整数。

    Returns:
        int: 2つの整数の合計。
    """
    # 合計を計算する
    result = a + b

    return result
```

## テスト方針

テストは pytest を使います。テストは `tests/` に置き、必要に応じて対象パッケージ構造に対応させます。ファイル名は `test_*.py`、テスト関数名は `test_entry_repository_lists_recent_entries` のように、検証内容が分かる名前にしてください。

`domain` ルール、use case、repository の振る舞いを優先して検証します。空入力、不正値、存在しないレコードなどの境界条件も含めます。バグ修正時は再発防止の regression test を追加してください。

## コミットと Pull Request

直近の履歴では `chore: uvで初期構築`、`chore: 環境準備` のような短い conventional-style の件名が使われています。コミット件名は簡潔にし、`chore:`、`feat:`、`fix:`、`docs:`、`test:` などの type prefix を付けます。

Pull Request には短い概要、関連 issue または ADR、実行したテスト結果を含めます。UI 変更ではスクリーンショットを添付してください。依存関係や設定を変えた場合は明記します。

## エージェント向け注意事項

既存の uv、Ruff、ty、pytest、pre-commit のワークフローを優先します。`uv.lock` は手で編集せず、uv コマンドで更新してください。変更は小さく保ち、ADR で定義されたアーキテクチャに沿わせます。
