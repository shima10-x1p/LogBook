# LogBook

Python 開発環境は `uv` を前提にしています。

## セットアップ

1. 依存関係を同期
	- `uv sync --group dev`
2. 開発用チェックを実行
	- `uv run ruff check .`
	- `uv run ruff format --check .`
	- `uv run ty check`
	- `uv run pytest`

## 日常的に使うコマンド

- 依存関係追加（本番）: `uv add <package>`
- 依存関係追加（開発）: `uv add --group dev <package>`
- テスト: `uv run pytest`
- Lint: `uv run ruff check .`
- Format: `uv run ruff format .`
- 型チェック: `uv run ty check`