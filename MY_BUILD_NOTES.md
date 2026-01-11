# OpenWrt 编译优化简介

## 硬件配置
| 项目 | 规格 |
| :-- | :-- |
| **设备** | 魔改亚瑟 (JDCloud RE-SS-01) |
| **芯片** | Qualcomm IPQ6010 |
| **内存** | 1GB DDR4 |
| **固件** | LiBwrt (main-nss) |
| **宽带** | 上海联通千兆 |

---

## 本次修改 (2026-01-11)

### `jdcloud_ipq60xx_libwrt.config`
| 修改 | 原值 | 新值 |
| :-- | :-- | :-- |
| 编译设备 | 4个全开 | 只保留 `jdcloud_re-ss-01` |
| zram-swap | `=m` | 已禁用 |
| OpenClash | 无 | `=y` |
| Nikki | 无 | `=y` (与 OpenClash 共存) |

### `nss.config`
| 修改 | 原值 | 新值 |
| :-- | :-- | :-- |
| IPQ 内存 | `MEM_PROFILE_256` | `MEM_PROFILE_1024` |
| ATH11K 内存 | `MEM_PROFILE_512M` | `MEM_PROFILE_1G` |

---

## 关键注意事项

### ⚠️ 与 NSS 冲突的功能（不要启用）
- `kmod-tcp-bbr` - NSS 绑过内核 TCP 栈
- `luci-app-turboacc` - 与 NSS 硬件加速冲突

### ✅ 原始项目已包含的功能
- Passwall、Mosdns、SmartDNS、SQM
- Docker 依赖（按需 opkg 安装）

---

## 触发编译
1. 前往 [GitHub Actions](https://github.com/Bernardxu123/wrt_release/actions)
2. 选择 **Build WRT** → **Run workflow**
3. 选择 `jdcloud_ipq60xx_libwrt`
