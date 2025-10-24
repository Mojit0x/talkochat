import React, { useState, useEffect } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet } from 'react-native';
import io from 'socket.io-client';

const socket = io('https://your-server-url-here'); // ← заміни на свій сервер, наприклад Render або Glitch

export default function App() {
  const [message, setMessage] = useState('');
  const [messages, setMessages] = useState([]);
  const [username, setUsername] = useState('');
  const [isLogged, setIsLogged] = useState(false);

  useEffect(() => {
    socket.on('chat message', (msg) => {
      setMessages((prev) => [...prev, msg]);
    });
    return () => socket.off('chat message');
  }, []);

  const sendMessage = () => {
    if (message.trim() !== '') {
      socket.emit('chat message', { username, text: message });
      setMessage('');
    }
  };

  if (!isLogged) {
    return (
      <View style={styles.loginContainer}>
        <Text style={styles.title}>TalkoChat 💬</Text>
        <TextInput
          style={styles.input}
          placeholder="Введи своє ім'я..."
          value={username}
          onChangeText={setUsername}
        />
        <Button title="Увійти" onPress={() => setIsLogged(true)} />
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <Text style={styles.header}>Привіт, {username} 👋</Text>
      <FlatList
        data={messages}
        keyExtractor={(_, i) => i.toString()}
        renderItem={({ item }) => (
          <Text style={styles.message}>
            <Text style={styles.sender}>{item.username}: </Text>
            {item.text}
          </Text>
        )}
      />
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Напиши повідомлення..."
          value={message}
          onChangeText={setMessage}
        />
        <Button title="➡️" onPress={sendMessage} />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  loginContainer: {
    flex: 1,
    backgroundColor: '#0a001a',
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
  container: {
    flex: 1,
    backgroundColor: '#0a001a',
    padding: 10,
  },
  title: {
    fontSize: 30,
    color: '#a060ff',
    marginBottom: 20,
  },
  header: {
    color: '#a060ff',
    fontSize: 20,
    marginVertical: 10,
    textAlign: 'center',
  },
  message: {
    color: '#fff',
    fontSize: 16,
    marginVertical: 3,
  },
  sender: {
    color: '#a060ff',
    fontWeight: 'bold',
  },
  inputContainer: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  input: {
    backgroundColor: '#1a0033',
    color: '#fff',
    flex: 1,
    borderRadius: 10,
    paddingHorizontal: 10,
    marginRight: 5,
  },
});
