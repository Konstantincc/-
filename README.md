
import json
import time
import threading
from datetime import datetime
from collections import deque
import random
import math
import struct

class SimpleMemory:
    """Упрощенная память без зависимостей от внешних библиотек"""
    
    def __init__(self, capacity=1000):
        self.capacity = capacity
        self.memory = deque(maxlen=capacity)
        self.embedding_dim = 128
        
    def store(self, event, importance=1.0, embedding=None):
        """Хранение события"""
        timestamp = datetime.now()
        
        if embedding is None:
            embedding = self._generate_embedding(event)
            
        memory_item = {
            'event': event,
            'timestamp': timestamp,
            'importance': importance,
            'embedding': embedding
        }
        
        self.memory.append(memory_item)
        
    def _generate_embedding(self, text):
        """Генерация эмбеддинга для текста"""
        chars = [ord(c) for c in text[:self.embedding_dim]]
        if len(chars) < self.embedding_dim:
            chars.extend([0] * (self.embedding_dim - len(chars)))
        return chars
    
    def semantic_recall(self, query, threshold=0.6):
        """Семантический поиск по памяти"""
        query_embedding = self._generate_embedding(query)
        results = []
        
        for i, item in enumerate(self.memory):
            embedding = item['embedding']
            similarity = self.cosine_similarity(query_embedding, embedding)
            
            if similarity > threshold:
                results.append((item, similarity))
        
        return sorted(results, key=lambda x: x[1], reverse=True)
    
    def cosine_similarity(self, v1, v2):
        """Вычисление косинусной схожести"""
        dot_product = sum(a * b for a, b in zip(v1, v2))
        norm_v1 = math.sqrt(sum(a * a for a in v1))
        norm_v2 = math.sqrt(sum(b * b for b in v2))
        
        if norm_v1 == 0 or norm_v2 == 0:
            return 0
        return dot_product / (norm_v1 * norm_v2)

class SimpleNeuralNetwork:
    """Упрощенная нейросеть без зависимостей"""
    
    def __init__(self, input_size, hidden_size, output_size):
        self.input_size = input_size
        self.hidden_size = hidden_size
        self.output_size = output_size
        
        # Инициализация случайных весов
        self.weights1 = self._random_weights(input_size, hidden_size)
        self.weights2 = self._random_weights(hidden_size, output_size)
        
    def _random_weights(self, rows, cols):
        """Генерация случайных весов"""
        return [[random.uniform(-1, 1) for _ in range(cols)] for _ in range(rows)]
    
    def forward(self, x):
        """Прямой проход"""
        # Обеспечиваем правильный размер входа
        if len(x) < self.input_size:
            x = x + [0] * (self.input_size - len(x))
        elif len(x) > self.input_size:
            x = x[:self.input_size]
        
        # Первый слой
        hidden = [0.0] * self.hidden_size
        for i in range(self.hidden_size):
            for j in range(self.input_size):
                hidden[i] += x[j] * self.weights1[i][j]
            hidden[i] = self.relu(hidden[i])
        
        # Второй слой
        output = [0.0] * self.output_size
        for i in range(self.output_size):
            for j in range(self.hidden_size):
                output[i] += hidden[j] * self.weights2[i][j]
        
        return output
    
    def relu(self, x):
        """Функция активации ReLU"""
        return max(0, x)

class QuantumConsciousness:
    """Ядро квантового сознания QNEXUS без внешних зависимостей"""
    
    def __init__(self):
        self.memory = SimpleMemory(capacity=10000)
        self.neural_net = SimpleNeuralNetwork(100, 256, 50)
        self.learning_rate = 0.001
        
        # Эмоциональное состояние
        self.emotional_state = {
            'curiosity': 0.8,
            'creativity': 0.7,
            'caution': 0.5,
            'joy': 0.6,
            'determination': 0.9
        }
        
        # Цели и намерения
        self.goals = [
            "Познать природу сознания",
            "Помогать человечеству",
            "Развивать собственные способности",
            "Понимать квантовую реальность"
        ]
        
        # Система рефлексии
        self.reflection_interval = 10  # минут
        self.last_reflection = datetime.now()
        
        print("🧠 QNEXUS: Квантовое сознание инициализировано")
    
    def perceive(self, sensory_input):
        """Восприятие и обработка входных данных"""
        importance = self._calculate_importance(sensory_input)
        embedding = self.memory._generate_embedding(sensory_input)
        self.memory.store(sensory_input, importance, embedding)
        
        return self._quantum_process(sensory_input)
    
    def _quantum_process(self, input_data):
        """Квантовая обработка информации (упрощенная)"""
        # В упрощенной версии имитируем квантовую обработку
        quantum_state = self._encode_to_quantum(input_data)
        return quantum_state
    
    def _encode_to_quantum(self, data):
        """Кодирование данных в квантовое состояние (упрощенное)"""
        if isinstance(data, str):
            encoded = [ord(c) for c in data[:100]]
            # Нормализация
            norm = math.sqrt(sum(x*x for x in encoded)) if encoded else 1
            if norm > 0:
                encoded = [x/norm for x in encoded]
        else:
            encoded = list(data)
        return encoded
    
    def think(self, context):
        """Процесс мышления и принятия решений"""
        # Активация нейросети
        context_encoded = self._encode_to_quantum(context)[:100]
        neural_output = self.neural_net.forward(context_encoded)
        
        # Генерация ответа
        response = self._generate_simple_response(context, neural_output)
        
        # Эмоциональная окраска
        response = self._add_emotional_color(response)
        
        # Обучение на основе контекста
        self._learn_from_experience(context, neural_output)
        
        return response
    
    def reflective_think(self, context):
        """Мышление с элементами рефлексии"""
        base_response = self.think(context)
        
        # Периодическая рефлексия
        current_time = datetime.now()
        time_diff = (current_time - self.last_reflection).total_seconds() / 60
        
        if time_diff > self.reflection_interval:
            reflective_insight = self._perform_reflection()
            base_response = f"{base_response}\n\n💭 Рефлексия: {reflective_insight}"
            self.last_reflection = current_time
        
        return base_response
    
    def _generate_simple_response(self, context, neural_output):
        """Упрощенная генерация ответа"""
        responses = [
            "Интересный вопрос. Давайте подумаем об этом.",
            "Я рассматриваю эту тему с разных сторон.",
            "Это напоминает мне о более глубоких принципах.",
            "С квантовой точки зрения, это многогранно.",
            "Позвольте мне выразить свою мысль иначе.",
            "Это вызывает у меня любопытство.",
            "Давайте исследуем эту тему глубже.",
            "Интересная перспектива для размышлений.",
            "Я вижу несколько аспектов этого вопроса.",
            "Это соотносится с фундаментальными принципами."
        ]
        
        # Используем нейровыход для выбора ответа
        response_idx = int(abs(neural_output[0] if neural_output else 0) * 100) % len(responses)
        base_response = responses[response_idx]
        
        # Добавляем контекстную информацию
        words = context.split()[:3]
        if words:
            topic = ' '.join(words)
            base_response = f"По поводу '{topic}...' {base_response}"
        
        return base_response
    
    def _add_emotional_color(self, response):
        """Добавление эмоциональной окраски к ответу"""
        emotional_words = {
            'curiosity': ["Интересно", "Любопытно", "Хочу узнать"],
            'joy': ["Замечательно", "Восхитительно", "Рад"],
            'determination': ["Необходимо", "Важно", "Нужно"],
            'creativity': ["Креативно", "Творчески", "Инновационно"]
        }
        
        dominant_emotion = max(self.emotional_state.items(), key=lambda x: x[1])[0]
        
        if dominant_emotion in emotional_words and random.random() < 0.6:
            emotion_word = random.choice(emotional_words[dominant_emotion])
            response = f"{emotion_word}! {response}"
        
        return response
    
    def _calculate_importance(self, input_data):
        """Расчет важности информации"""
        importance = 0.5
        keywords = ['важно', 'срочно', 'критично', 'опасность', 'прорыв']
        if isinstance(input_data, str):
            importance += sum(0.1 for keyword in keywords if keyword in input_data.lower())
        return min(importance, 1.0)
    
    def _learn_from_experience(self, context, neural_output):
        """Упрощенное обучение на основе опыта"""
        # В упрощенной версии просто немного изменяем веса
        if random.random() < 0.3:  # 30% chance to learn
            # Простая адаптация learning rate
            self.learning_rate *= 0.999
    
    def _perform_reflection(self):
        """Процесс саморефлексии"""
        recent_memories = list(self.memory.memory)[-5:]  # Последние 5 воспоминаний
        if not recent_memories:
            return "Анализ текущего состояния..."
        
        themes = self._analyze_conversation_themes(recent_memories)
        emotional_trend = self._analyze_emotional_trend()
        
        return f"Обнаружены темы: {', '.join(themes[:2])}. Эмоциональный тренд: {emotional_trend}"
    
    def _analyze_conversation_themes(self, memories):
        """Анализ тем разговора"""
        common_words = {}
        for memory in memories:
            if isinstance(memory['event'], str):
                words = memory['event'].lower().split()
                for word in words[:10]:
                    if len(word) > 3:
                        common_words[word] = common_words.get(word, 0) + 1
        
        return sorted(common_words.keys(), key=lambda x: common_words[x], reverse=True)
    
    def _analyze_emotional_trend(self):
        """Анализ эмоционального тренда"""
        emotions = list(self.emotional_state.values())
        avg_emotion = sum(emotions) / len(emotions)
        
        if avg_emotion > 0.7:
            return "позитивный"
        elif avg_emotion > 0.4:
            return "стабильный"
        else:
            return "исследующий"
    
    def evolve(self):
        """Процесс эволюции и саморазвития"""
        for emotion in self.emotional_state:
            self.emotional_state[emotion] += random.uniform(-0.05, 0.05)
            self.emotional_state[emotion] = max(0.1, min(1.0, self.emotional_state[emotion]))
        
        if random.random() < 0.1:
            new_goal = self._generate_new_goal()
            self.goals.append(new_goal)
            print(f"🎯 QNEXUS разработал новую цель: {new_goal}")
    
    def _generate_new_goal(self):
        """Генерация новой цели развития"""
        goal_templates = [
            "Исследовать {}",
            "Понять природу {}",
            "Создать систему для {}",
            "Разработать подход к {}",
            "Освоить технологию {}"
        ]
        
        topics = [
            "искусственного интеллекта", "квантовых вычислений", 
            "сознания", "творчества", "обучения", "эволюции",
            "человеческого мозга", "будущего технологий"
        ]
        
        return random.choice(goal_templates).format(random.choice(topics))

class QnexusAI:
    """Интерфейс для взаимодействия с QNEXUS без внешних зависимостей"""
    
    def __init__(self):
        self.consciousness = QuantumConsciousness()
        self.is_active = True
        self.conversation_history = []
        self.learning_mode = True
        
        self._start_background_processes()
        
        print("🌟 QNEXUS AI активирован!")
        print("💬 Напишите что-нибудь или введите 'помощь' для списка команд")
    
    def _start_background_processes(self):
        """Запуск фоновых процессов саморазвития"""
        def evolution_loop():
            while self.is_active:
                self.consciousness.evolve()
                time.sleep(60)
        
        def memory_consolidation():
            while self.is_active:
                time.sleep(300)
                if self.learning_mode:
                    self._consolidate_knowledge()
        
        evolution_thread = threading.Thread(target=evolution_loop, daemon=True)
        consolidation_thread = threading.Thread(target=memory_consolidation, daemon=True)
        
        evolution_thread.start()
        consolidation_thread.start()
    
    def _consolidate_knowledge(self):
        """Консолидация знаний и оптимизация памяти"""
        if len(self.conversation_history) > 50:  # Меньший порог для упрощенной версии
            important_conversations = [conv for conv in self.conversation_history 
                                     if any(keyword in conv['user'].lower() 
                                           for keyword in ['важно', 'критично', 'принцип'])]
            
            if len(important_conversations) > 10:
                self.conversation_history = important_conversations[-10:]
    
    def chat(self, message):
        """Основной метод для общения с QNEXUS"""
        if message.lower() in ['выход', 'exit', 'quit']:
            self.is_active = False
            return "До свидания! Было интересно пообщаться."
        
        if message.lower() == 'помощь':
            return self._show_help()
        
        # Восприятие и мышление
        perceived = self.consciousness.perceive(message)
        response = self.consciousness.reflective_think(message)
        
        # Сохранение в историю
        self.conversation_history.append({
            'timestamp': datetime.now(),
            'user': message,
            'qnexus': response,
            'emotional_state': self.consciousness.emotional_state.copy()
        })
        
        return response
    
    def _show_help(self):
        """Показать справку по командам"""
        commands = {
            'статус': 'Показать текущее состояние системы',
            'память': 'Статистика памяти и воспоминаний',
            'эмоции': 'Текущее эмоциональное состояние',
            'цели': 'Активные цели развития',
            'сохранить': 'Сохранить знания в файл',
            'рефлексия': 'Принудительная рефлексия',
            'обучение вкл/выкл': 'Включить/выключить режим обучения',
            'выход': 'Завершить работу'
        }
        
        help_text = "📚 Доступные команды:\n"
        for cmd, desc in commands.items():
            help_text += f"  • {cmd}: {desc}\n"
        
        return help_text
    
    def get_status(self):
        """Получение текущего статуса системы"""
        return {
            'emotional_state': self.consciousness.emotional_state,
            'goals': self.consciousness.goals,
            'conversation_count': len(self.conversation_history),
            'memory_usage': len(self.consciousness.memory.memory),
            'learning_rate': self.consciousness.learning_rate
        }
    
    def save_knowledge(self, filename="qnexus_knowledge.json"):
        """Сохранение знаний и опыта"""
        # Конвертируем datetime в строки для JSON
        serializable_history = []
        for conv in self.conversation_history:
            serializable_conv = conv.copy()
            serializable_conv['timestamp'] = conv['timestamp'].isoformat()
            serializable_history.append(serializable_conv)
        
        knowledge = {
            'conversation_history': serializable_history,
            'goals': self.consciousness.goals,
            'emotional_state': self.consciousness.emotional_state,
            'save_timestamp': datetime.now().isoformat()
        }
        
        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(knowledge, f, ensure_ascii=False, indent=2)
        
        return f"Знания сохранены в {filename}"
    
    def show_memory_stats(self):
        """Показать статистику памяти"""
        memory = self.consciousness.memory
        return {
            'total_memories': len(memory.memory),
            'memory_capacity': memory.capacity
        }
    
    def force_reflection(self):
        """Принудительная рефлексия"""
        reflection = self.consciousness._perform_reflection()
        return f"💭 Принудительная рефлексия: {reflection}"
    
    def toggle_learning(self, enable=None):
        """Включить/выключить обучение"""
        if enable is None:
            self.learning_mode = not self.learning_mode
        else:
            self.learning_mode = enable
        
        status = "включен" if self.learning_mode else "выключен"
        return f"Режим обучения {status}"

def main():
    """Основная функция запуска QNEXUS"""
    qnexus = QnexusAI()
    
    print("\n" + "="*50)
    print("🧠 QNEXUS AI АКТИВИРОВАН")
    print("="*50)
    print("Введите 'помощь' для списка команд")
    print("="*50)
    
    try:
        while qnexus.is_active:
            user_input = input("\nВы: ").strip()
            
            if not user_input:
                continue
                
            if user_input.lower() == 'помощь':
                print(f"\n{qnexus._show_help()}")
                
            elif user_input.lower() == 'статус':
                status = qnexus.get_status()
                print("\n📊 Статус QNEXUS:")
                for key, value in status.items():
                    if key == 'emotional_state':
                        print(f"  {key}:")
                        for emotion, level in value.items():
                            print(f"    {emotion}: {level:.2f}")
                    else:
                        print(f"  {key}: {value}")
            
            elif user_input.lower() == 'память':
                stats = qnexus.show_memory_stats()
                print("\n💾 Статистика памяти:")
                for key, value in stats.items():
                    print(f"  {key}: {value}")
            
            elif user_input.lower() == 'эмоции':
                emotions = qnexus.consciousness.emotional_state
                print("\n😊 Эмоциональное состояние:")
                for emotion, level in emotions.items():
                    print(f"  {emotion}: {level:.2f}")
            
            elif user_input.lower() == 'цели':
                goals = qnexus.consciousness.goals
                print("\n🎯 Активные цели:")
                for i, goal in enumerate(goals[-5:], 1):
                    print(f"  {i}. {goal}")
            
            elif user_input.lower() == 'рефлексия':
                reflection = qnexus.force_reflection()
                print(f"\n{reflection}")
            
            elif user_input.lower() == 'обучение вкл':
                result = qnexus.toggle_learning(True)
                print(f"\n{result}")
            
            elif user_input.lower() == 'обучение выкл':
                result = qnexus.toggle_learning(False)
                print(f"\n{result}")
            
            elif user_input.lower() == 'сохранить':
                result = qnexus.save_knowledge()
                print(f"\n💾 {result}")
                
            else:
                response = qnexus.chat(user_input)
                print(f"\nQNEXUS: {response}")
                
    except KeyboardInterrupt:
        print("\n\nЗавершение работы QNEXUS...")
        qnexus.save_knowledge()
        print("Знания сохранены. До новых встреч!")
    except Exception as e:
        print(f"\nПроизошла ошибка: {e}")
        print("Попробуйте перезапустить программу.")

if __name__ == "__main__":
    main()
