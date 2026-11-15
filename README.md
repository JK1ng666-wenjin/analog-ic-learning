# Week 02 · MOS 输出特性实验

## 目的

1. 验证饱和区平方律：\(I_D \propto (V_{GS}-V_{TH})^2\)；
2. 验证 \(I_D \propto W/L\)；
3. 观察沟道长度调制 \(\lambda\) 对饱和区斜率的影响。

## 电路与参数

- 器件：NMOS，Level 1 模型（教学用平方律模型）
- \(V_{TH}=0.7\ \text{V}\)，\(KP=120\ \mu\text{A/V}^2\)，\(L=1\ \mu\text{m}\)
- 基准：\(W=10\ \mu\text{m}\)，\(V_{GS}=2\ \text{V}\)，\(V_{DS}\) 从 0 扫到 3 V

## 结果

| 实验 | 条件 | 结果 |
|---|---|---|
| 基准 | \(W=10\,\mu m\)，\(\lambda=0.02\) | 拐点 \(V_{DS}=1.3\ \text{V}\)，平台电流 \(1.05\ \text{mA}\) |
| 改宽 | \(W=40\,\mu m\)，\(\lambda=0.02\) | 平台电流 \(4.2\ \text{mA}\)，为基准的 4 倍 |
| 改 λ | \(W=10\,\mu m\)，\(\lambda=0.05\) | 1.3 V→3.0 V 涨幅由 3.3% 升到 8.0% |

![MOS 输出特性与 λ 对比](week02_summary.png)

> 图由本人 ngspice 仿真数据绘制（WSLg 图形窗口渲染异常，截图不可用）。

## 三个结论

1. **平方律成立**：拐点出现在 \(V_{DS}=V_{GS}-V_{TH}=1.3\ \text{V}\)，与理论一致；平台电流量级与 \(I_D=\frac{1}{2}\mu C_{ox}\frac{W}{L}(V_{GS}-V_{TH})^2\) 相符。
2. **\(I_D \propto W/L\)**：\(W\) 从 10 μm 增到 40 μm，电流从 1.05 mA 增到 4.2 mA，比值 4.0，验证宽长比的线性关系。
3. **λ 只改饱和区**：\(\lambda\) 从 0.02 增到 0.05，线性区曲线重合，饱和区斜率变陡；涨幅由 3.3% 增到 8.0%，对应输出电阻 \(r_o=1/(\lambda I_D)\) 下降。

## 复现

```bash
cd ~/ic/week02
ngspice -b mos_one.cir      # 基准曲线
ngspice -b mos_w40.cir      # W=40um
ngspice -b mos_lam05.cir    # lambda=0.05
python3 plot_week02.py      # 生成总结图
```

## 踩坑记录

- 初版网表把电流测在 `VDD` 上，但 `VDD` 未接电路，读到全 0，ngspice 窗口空白；
- 改为测 `-i(VDS)`（实际供电的电压源）后曲线正常。
