# マルチカム音声同期 — 相互相関による自動オフセット計測

対象: FX30 (Mov) + iPhone (MP4) のような2カメラ現場で、クラップ板なしに音声から同期点を探す場合。

## 基本手順

1. 両ファイルから音声を抽出 (モノラル 48kHz PCM)
2. **エンベロープ相互相関**でオフセット確定
3. ドリフト (クロック差) を全長コーパスで補正

```bash
# 音声抽出
ffmpeg -i input.mov -map 0:a:0 -ac 1 -ar 48000 -c:a pcm_s16le mov.wav
ffmpeg -i input.mp4 -map 0:a:0 -ac 1 -ar 48000 -c:a pcm_s16le mp4.wav
```

```python
import numpy as np, wave

def load(p):
    w = wave.open(p, 'rb')
    a = np.frombuffer(w.readframes(w.getnframes()), dtype='<i2').astype(np.float64)
    return a / 32768, w.getframerate()

def envelope(a, sr, window_ms=50):
    """Hilbert法の代替: 短時間RMSエンベロープ"""
    n = int(sr * window_ms / 1000)
    return np.array([np.sqrt(np.mean(a[i:i+n]**2)) for i in range(0, len(a)-n, n//2)])

def xcorr_offset(a, b, sr_env):
    """全長相関でオフセット確定 (サンプル単位)"""
    n = len(a) + len(b) - 1
    A = np.fft.rfft(a, n)
    B = np.fft.rfft(b, n)
    corr = np.fft.irfft(A * np.conj(B))
    lag = np.argmax(corr)
    if lag > n // 2:
        lag -= n
    return lag
```

## 落とし穴

**PHAT (Phase Transform) は使わない**
- ラグ 0 にアーティファクトが出やすく、全長相関でゼロ付近に擬似ピークが立つ
- 音楽素材では素のエンベロープ相関の方が安定する

**窓幅はビート周期より小さくする**
- ビート周期 (例: 0.25〜1秒) より広い窓で分割計測すると周期スリップが発生する
- 最初に全長相関でオフセット候補を絞り、その後 ±(ビート周期/4) 幅で精密化する

**クロックドリフトを無視しない**
- カメラ間のクロック差は -100〜+100 ppm 程度ある
- 全長 10〜30 分なら 60〜180 ms のドリフト = フレーム2〜5枚分のズレ
- 補正: `ffmpeg -i mov.wav -af "asetrate=48000*(1+drift_ppm/1e6)" corrected.wav`

## 実例 (プペル number/琴、2026-09-04)

| 項目 | 値 |
|------|-----|
| カメラ A | FX30 → `琴_夜_number.mov` (24/30fps VFR) |
| カメラ B | iPhone → `琴_夜_寄り.MP4` |
| 手順書実測 | FX30 が 11.4s 先行 (色合わせ手順.html) |
| 相互相関結果 | **11.402〜11.423 s** (窓 21 個の残差 4 ms) |
| クロックドリフト | **-97.7 ppm** (全長 -26 ms) |
| 使用手法 | エンベロープ (短時間 RMS) × 全長相関 |

ビート周期 (0.25s) が原因で 11.16s という誤ピークが出たが、全長相関で 11.41s に確定した。

## 次回同じことをするときのチェックリスト

1. 手順書/クラップ板の実測値が既にある → それを初期値として ±2s の探索で十分
2. 音楽素材 → PHAT は使わずエンベロープ相関
3. 全長相関 → 分割窓で検証 (ビート周期の 1/4 以下の窓幅)
4. ドリフト補正: 全長で測定、素材が長いほど重要
5. Mov が VFR の場合は ffprobe で frame PTS を出して実フレームレートを確認

---

✅ うまく行ったこと
- 手順書実測 11.4s と相互相関結果 11.41s が 10ms 精度で一致
- 全長相関でビートスリップ候補 (11.16s) を排除できた

❌ 詰まったこと
- PHAT: ラグ 0 アーティファクトで全長相関が失敗 → エンベロープ相関に切り替え
- 分割窓: ビート周期 (0.25s/0.992s) に相関が引っ張られてスリップ → 窓幅を狭めてから全長決着

📋 次回同じことをするときのチェックリスト
- 上記「チェックリスト」参照
