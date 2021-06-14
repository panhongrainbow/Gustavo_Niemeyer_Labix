# Vclock 执行说明

## 快速执行

```bash
$ cd /home/panhong/go/src/github.com/panhongrainbow/Gustavo_Niemeyer_Labix

$ go test -v ./
# === RUN   TestAll
# &{true [{g1 2 11} {g3 2 0} {g2 2 0}]}
# &{true [{g2 2 3} {g1 1 0}]}
# &{true [{g3 2 3} {g2 2 0} {g1 1 0}]}
# OK: 12 passed
# --- PASS: TestAll (0.00s)
# PASS
# ok      github.com/panhongrainbow/Gustavo_Niemeyer_Labix        0.002s
```

## 2021年6月15日新增

开始在档案 /home/panhong/go/src/github.com/panhongrainbow/Gustavo_Niemeyer_Labix/vclock/vclock_test.go 最下面开始新增测试

```go
// 以下为新增测试

// 新增测试1：测试 Vector Clock Join 到原来的结点
func (s *S) TestMergeBack1(c *C) {
	{
		vc1 := vclock.New() // Go Routine g1 的 Vector Clock
		vc1.Update("g1", 4)

		vc2 := vclock.New() // Go Routine g2 的 Vector Clock
		vc2.Update("g2", 1)
		vc2.Merge(vc1)
		vc2.Update("g2", 3)

		vc3 := vclock.New() // Go Routine g3 的 Vector Clock
		vc3.Update("g3", 1)
		vc3.Merge(vc2)
		vc3.Update("g3", 3)

		vc1.Update("g1", 11)

		vc1.Merge(vc3)
		fmt.Println(vc1)
		fmt.Println(vc2)
		fmt.Println(vc3)
	}
}
```









