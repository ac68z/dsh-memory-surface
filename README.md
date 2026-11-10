# dsh-memory-surface

每轮第一步（`agent/pre-step`）拿刚进来的那句话去本地记忆浮现服务（默认 `127.0.0.1:8799`）
问一次「最像的旧记忆」，命中的两三条会作为带来源的 `user/message` 折进本轮上下文，
在会话里显示成一段 `<memory-surface>` 系统提醒。

- 只查**同一个会话**的记忆（session id 传给服务端过滤），不同会话不串门。
- 服务不在 / 超时 / 没有够像的（默认余弦 < 0.52）→ 安静跳过，绝不影响这一轮。
- 同一句话只浮现一次（文本指纹去重）。
- 服务脚本：`qq-bridge/state/agents/star-map/memory_surface_daemon.py`（模型 bge-small-zh，常驻 CPU）。
- 环境变量：`DSH_MEMORY_SURFACE_PORT`（默认 8799）、`DSH_MEMORY_SURFACE_MIN`（默认 0.52）。