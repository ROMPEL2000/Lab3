# 🧮 Асинхронное вычисление числа π с использованием формулы Грегори-Лейбница

Этот проект демонстрирует асинхронное вычисление числа π (пи) с использованием **формулы Грегори-Лейбница** и возможностью отмены задачи через `CancellationToken`. Пример полезен для изучения концепций асинхронного программирования в C#, включая обработку задач с задержками и управление отменой.

---

## 🖋️ Описание

Задача вычисляет приближенное значение числа π с использованием **ряда Грегори-Лейбница**:

Каждое вычисление выполняется в отдельной итерации с выводом текущего значения числа π. Асинхронная задержка (`Task.Delay`) используется для симуляции длительной операции.

### Особенности:
- **Асинхронное выполнение:** Все вычисления выполняются без блокировки основного потока.
- **Обработка отмены:** Поддержка `CancellationToken` для прерывания задачи.
- **Динамический вывод:** Выводит номер итерации и текущее значение числа π.

---

### Код: 
```﻿using System;
using System.Threading;
using System.Threading.Tasks;

namespace DistributedQueue.Common
{
    internal class GregoryLeibnizGetPIJob : IComputePiJob
    {
        public async Task ComputePyAsync(string name, int iterations, CancellationToken token)
        {
            var startTime = DateTime.Now;

            double pi = 0.0;
            int sign = 1;

            Console.WriteLine($"Начало задачи: {name}. Итерации: {iterations}");

            for (int i = 0; i < iterations; i++)
            {
                // Проверяем, был ли запрос на отмену
                if (token.IsCancellationRequested)
                {
                    Console.WriteLine($"Задача {name} отменена.");
                    break;
                }

                // Грегори-Лейбниц формула для вычисления π
                pi += sign * (1.0 / (2 * i + 1));
                sign *= -1;

                Console.WriteLine($"{DateTime.Now}: Задача {name} — Итерация {i + 1}, Текущее значение pi: {pi * 4}");

                // Задержка для демонстрации асинхронной работы
                await Task.Delay(1000, token);
            }

            var endTime = DateTime.Now;
            Console.WriteLine($"Завершение задачи: {name}. Итераций: {iterations}. Итоговое значение π: {pi * 4}");
            Console.WriteLine($"Задача выполнена за {(endTime - startTime).TotalSeconds} секунд.");
        }
    }
}
```

---

### Пример работы:
![image](https://github.com/user-attachments/assets/5ca739c9-8301-4682-ac9f-62e7463d65be)
