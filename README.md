import React, { useEffect, useState } from 'react';
import {
  View,
  Text,
  TextInput,
  Button,
  FlatList,
  TouchableOpacity,
  Alert,
  StyleSheet,
  ScrollView,
  Picker
} from 'react-native';

import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

/* ================= LOGIN SCREEN ================= */
const LoginScreen = ({ navigation }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = () => {
    if (email && password) {
      navigation.navigate('JobList');
    } else {
      Alert.alert('Login Failed', 'Enter valid email & password');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login</Text>
      <TextInput
        placeholder="Email"
        value={email}
        onChangeText={setEmail}
        style={styles.input}
        keyboardType="email-address"
        autoCapitalize="none"
      />
      <TextInput
        placeholder="Password"
        value={password}
        onChangeText={setPassword}
        style={styles.input}
        secureTextEntry
      />
      <Button title="Login" onPress={handleLogin} />
      <View style={{ marginTop: 10 }}>
        <Button title="Signup" onPress={() => navigation.navigate('Signup')} />
      </View>
      <TouchableOpacity
        onPress={() => navigation.navigate('ForgotPassword')}
        style={{ marginTop: 15 }}
      >
        <Text style={{ color: 'blue', textAlign: 'center' }}>Forgot Password?</Text>
      </TouchableOpacity>
    </View>
  );
};

/* ================= SIGNUP SCREEN ================= */
const SignupScreen = ({ navigation }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSignup = () => {
    if (email && password) {
      Alert.alert('Signup Successful', 'Account created successfully!');
      navigation.goBack();
    } else {
      Alert.alert('Error', 'Enter valid details');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Signup</Text>
      <TextInput
        placeholder="Email"
        value={email}
        onChangeText={setEmail}
        style={styles.input}
      />
      <TextInput
        placeholder="Password"
        value={password}
        onChangeText={setPassword}
        style={styles.input}
        secureTextEntry
      />
      <Button title="Create Account" onPress={handleSignup} />
    </View>
  );
};

/* ================= FORGOT PASSWORD SCREEN ================= */
const ForgotPasswordScreen = ({ navigation }) => {
  const [email, setEmail] = useState('');

  const handleReset = () => {
    if (email) {
      Alert.alert('Password Reset', 'Password reset link sent to your email');
      navigation.goBack();
    } else {
      Alert.alert('Error', 'Enter your email');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Forgot Password</Text>
      <TextInput
        placeholder="Enter your email"
        value={email}
        onChangeText={setEmail}
        style={styles.input}
      />
      <Button title="Reset Password" onPress={handleReset} />
    </View>
  );
};

/* ================= JOB LIST SCREEN ================= */
const JobListScreen = ({ navigation }) => {
  const allJobs = [
    {
      title: 'Software Engineer',
      description: 'Develop applications',
      requirements: { experience: 3, education: ['BS Computer Science', 'BS IT'] },
      location: 'Islamabad',
      field: 'Computer',
    },
    {
      title: 'Web Developer',
      description: 'Build websites using HTML, CSS, JS',
      requirements: { experience: 2, education: ['BS Computer Science', 'BS IT'] },
      location: 'Islamabad',
      field: 'Computer',
    },
    {
      title: 'Network Engineer',
      description: 'Manage company networks',
      requirements: { experience: 5, education: ['BS IT', 'BS Computer Science'] },
      location: 'Lahore',
      field: 'Computer',
    },
    {
      title: 'Data Analyst',
      description: 'Analyze data sets',
      requirements: { experience: 1, education: ['BS Computer Science', 'BS Statistics'] },
      location: 'Islamabad',
      field: 'Computer',
    },
    {
      title: 'Cyber Security Analyst',
      description: 'Secure company systems',
      requirements: { experience: 4, education: ['BS IT', 'BS Computer Science'] },
      location: 'Islamabad',
      field: 'Computer',
    },
    {
      title: 'Mobile App Developer',
      description: 'Develop mobile applications',
      requirements: { experience: 3, education: ['BS Computer Science'] },
      location: 'Karachi',
      field: 'Computer',
    },
  ];

  // Filter jobs for Islamabad & Computer field
  const jobs = allJobs.filter(j => j.location === 'Islamabad' && j.field === 'Computer');

  return (
    <View style={{ flex: 1, padding: 20 }}>
      <Text style={styles.title}>Job Listings - Islamabad (Computer Field)</Text>
      <FlatList
        data={jobs}
        keyExtractor={(item, index) => index.toString()}
        renderItem={({ item }) => (
          <TouchableOpacity onPress={() => navigation.navigate('JobDetails', { job: item })}>
            <View style={styles.jobItem}>
              <Text style={styles.jobTitle}>{item.title}</Text>
              <Text style={styles.jobLocation}>{item.location}</Text>
            </View>
          </TouchableOpacity>
        )}
      />
    </View>
  );
};

/* ================= JOB DETAILS SCREEN ================= */
const JobDetailsScreen = ({ navigation, route }) => {
  const { job } = route.params;
  const [experience, setExperience] = useState('');
  const [education, setEducation] = useState('');

  const handleApply = () => {
    const requiredExp = job.requirements.experience;
    if (parseInt(experience) < requiredExp) {
      Alert.alert('Error', `Minimum experience required is ${requiredExp} years`);
      return;
    }
    if (!education) {
      Alert.alert('Error', 'Please select your education');
      return;
    }
    Alert.alert('Success', 'Application Submitted Successfully!');
    navigation.goBack();
  };

  return (
    <ScrollView style={{ flex: 1, padding: 20 }}>
      <Text style={styles.jobTitle}>{job.title}</Text>
      <Text style={styles.jobDescription}>{job.description}</Text>
      <Text style={styles.jobText}>Location: {job.location}</Text>
      <Text style={styles.jobText}>Minimum Experience: {job.requirements.experience} years</Text>
      <Text style={styles.jobText}>Select Your Education:</Text>

      <Picker
        selectedValue={education}
        style={{ height: 50, marginBottom: 15 }}
        onValueChange={(itemValue) => setEducation(itemValue)}
      >
        <Picker.Item label="Select Education" value="" />
        {job.requirements.education.map((edu, index) => (
          <Picker.Item label={edu} value={edu} key={index} />
        ))}
      </Picker>

      <TextInput
        placeholder="Enter your experience (years)"
        value={experience}
        onChangeText={setExperience}
        keyboardType="numeric"
        style={styles.input}
      />

      <Button title="Apply Now" onPress={handleApply} />
      <View style={{ marginTop: 15 }}>
        <Button title="Back" onPress={() => navigation.goBack()} />
      </View>
    </ScrollView>
  );
};

/* ================= APP ENTRY ================= */
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Login">
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Signup" component={SignupScreen} />
        <Stack.Screen name="ForgotPassword" component={ForgotPasswordScreen} />
        <Stack.Screen name="JobList" component={JobListScreen} />
        <Stack.Screen name="JobDetails" component={JobDetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

/* ================= STYLES ================= */
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    padding: 20,
    backgroundColor: '#fff',
  },
  title: {
    fontSize: 22,
    fontWeight: 'bold',
    marginBottom: 20,
    textAlign: 'center',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    borderRadius: 8,
    marginBottom: 15,
  },
  jobItem: {
    padding: 12,
    borderBottomWidth: 1,
    borderColor: '#ddd',
    marginBottom: 5,
  },
  jobTitle: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  jobLocation: {
    fontSize: 14,
    color: 'gray',
  },
  jobDescription: {
    fontSize: 16,
    marginTop: 10,
  },
  jobText: {
    fontSize: 16,
    marginTop: 10,
  },
});
