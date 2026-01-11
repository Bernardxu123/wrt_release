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

## 配置修改 (2026-01-11)

### 核心修复 (参照 breeze303 成功配置)
| 改动 | 新值 | 原因 |
| :-- | :-- | :-- |
| UPnP | `miniupnpd-nftables=y` | 旧 miniupnpd 已废弃 |
| LuCI | 显式声明 `luci=y`, `luci-base=y` | 确保依赖链完整 |
| 防火墙 | `luci-app-firewall=y` | 显式依赖 |

### 模块化包 (避免依赖冲突)
| 包 | 设置 | 原因 |
| :-- | :-- | :-- |
| samba4 | `=m` | libncursesw6 |
| diskman | `=m` | libncursesw6 |
| quickfile | `=m` | nginx |
| Docker 相关 | `=m` | 按需加载 |

### 移除的包 (依赖不可用)
- `usbutils` / `usbmuxd` (libudev)

### `proxy.config` 代理应用
- Passwall (默认)
- OpenClash (已添加)
- Nikki (已添加)

### `nss.config` 内存优化
| 配置 | 值 |
| :-- | :-- |
| IPQ 内存 | `MEM_PROFILE_1024` |
| ATH11K 内存 | `MEM_PROFILE_1G` |

---

## ⚠️ 注意事项

### 与 NSS 冲突 (不要启用)
- `kmod-tcp-bbr`
- `luci-app-turboacc`

### LiBwrt 不可用的依赖
- `libudev` → usbutils, usbmuxd
- `libncursesw6` → diskman (模块化)
- `miniupnpd` → 改用 `miniupnpd-nftables`

---

## 触发编译
1. [GitHub Actions](https://github.com/Bernardxu123/wrt_release/actions)
2. **Build WRT** → **Run workflow**
3. 选择 `jdcloud_ipq60xx_libwrt`
