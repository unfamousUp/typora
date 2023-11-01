# Bukkit插件开发

## 事件处监听器/处理器

### 1. 创建事件监听器

> Bukkit的Listener接口

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.bukkit.event;

public interface Listener {
}

```

> 用户自定义的事件监听器器EventListener

```java
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerMoveEvent;

public class EventListener implements Listener {
    // 通常用于标记一个方法作为事件处理器
    @EventHandler
    public void dontMove(PlayerMoveEvent e) { // 方法类型，即代表我们要监听的事件
        double distance = e.getFrom().distance(e.getTo());
        if (distance != 0) {
            e.setCancelled(true);
        }
    }
}
```

### 2. 注册事件处理器

## 命令处理器



