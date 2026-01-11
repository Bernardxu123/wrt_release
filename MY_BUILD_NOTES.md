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

## 本次修改

### `jdcloud_ipq60xx_libwrt.config`
| 修改 | 新值 | 原因 |
| :-- | :-- | :-- |
| 编译设备 | 只保留 `jdcloud_re-ss-01` | 只需亚瑟 |
| zram-swap | `=n` | 1GB 不需要 |
| usbmuxd/usbutils | `=n` | libudev 不可用 |
| luci-app-diskman | `=n` | libncursesw6 不可用 |
| luci-app-quickfile | `=n` | nginx 不可用 |
| luci-app-upnp | `=n` | miniupnpd 不可用 |

### `proxy.config` (代理应用集中管理)
| 包 | 状态 |
| :-- | :-- |
| Passwall | `=y` (项目默认) |
| OpenClash | `=y` (已添加) |
| Nikki | `=y` (已添加) |

### `nss.config`
| 修改 | 新值 |
| :-- | :-- |
| IPQ 内存 | `MEM_PROFILE_1024` |
| ATH11K 内存 | `MEM_PROFILE_1G` |

---

## ⚠️ LiBwrt 中不可用的依赖 (别尝试启用这些)
- `libudev` → usbutils, usbmuxd
- `libncursesw6` → diskman
- `nginx` / `uci-firewall` → quickfile
- `miniupnpd` → upnp

## ⚠️ 与 NSS 冲突的功能 (别启用)
- `kmod-tcp-bbr`
- `luci-app-turboacc`

---

## 触发编译
1. 前往 [GitHub Actions](https://github.com/Bernardxu123/wrt_release/actions)
2. 选择 **Build WRT** → **Run workflow**
3. 选择 `jdcloud_ipq60xx_libwrt`
