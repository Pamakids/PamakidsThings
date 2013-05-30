Redis常用命令
---

Command | Result
--- | ---
Keys * | MSET one 1 two 2 three 3 four 4 <br> `OK` <br> KYES \*o* <br> `"two","four","one"`<br>KEYS t??<br>`"two"`<br>KEYS *<br> `"one", "two","three","four"`
ttl "" | 查看当前值到期时间
expire key 10 | 设置key过期时间
del key | 删除key，返回删除数量