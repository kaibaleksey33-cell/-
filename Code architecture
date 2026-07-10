class AdaptiveCPU:
    def __init__(self):
        self.current_bus_width = 64
        self.pc = 0  
        self.call_stack = []
        
        # Теперь индексы строгие:
        # 0: ADD, 1: CALL_EXT, 2: SUB, 3: HALT, 4: VECTOR_MUL, 5: RET
        self.memory = [
            ("ADD", "simple_data"),       # 0: Базовый режим
            ("CALL_EXT", (4, 512)),       # 1: Прыгаем на ИНДЕКС 4 (VECTOR_MUL) и включаем 512 бит
            ("SUB", "simple_data"),       # 2: Возврат сюда (64 бит)
            ("HALT", None),               # 3: Конец программы
            ("VECTOR_MUL", "heavy_data"), # 4: Сама функция (выполняется на 512 бит)
            ("RET", None)                 # 5: Возврат
        ]

    def log_state(self, action):
        print(f"[PC: {self.pc}] {action:<25} | Шина: {self.current_bus_width} бит")

    def run(self):
        print("=== ЗАПУСК СИМУЛЯТОРА АДАПТИВНОЙ АРХИТЕКТУРЫ ===")
        
        while True:
            if self.pc >= len(self.memory):
                print("Ошибка: Выход за пределы памяти.")
                break
                
            command, args = self.memory[self.pc]
            
            if command == "HALT":
                self.log_state("Остановка (HALT)")
                break
                
            elif command == "CALL_EXT":
                target_pc, target_width = args
                # Сохраняем адрес возврата (pc + 1) и текущую разрядность
                self.call_stack.append((self.pc + 1, self.current_bus_width))
                
                # Меняем разрядность шины и прыгаем
                self.current_bus_width = target_width
                self.log_state(f"CALL на адрес {target_pc}")
                self.pc = target_pc
                continue 
                
            elif command == "RET":
                if not self.call_stack:
                    print("Ошибка: Стек пуст.")
                    break
                # Возвращаем старый адрес и старую разрядность
                return_pc, previous_width = self.call_stack.pop()
                self.current_bus_width = previous_width
                self.log_state("RET (Откат шины)")
                self.pc = return_pc
                continue
                
            else:
                self.log_state(f"Выполнение {command}")
                
            self.pc += 1
        print("=== СИМУЛЯЦИЯ ЗАВЕРШЕНА ===")

if __name__ == "__main__":
    cpu = AdaptiveCPU()
    cpu.run()
