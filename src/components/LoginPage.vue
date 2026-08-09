<template>
  <div class="login-container">
    <!-- Background gradient animation -->
    <div class="animated-background"></div>
    
    <!-- Login card with glassmorphism effect -->
    <div class="login-card">
      <div class="card-header">
        <h1 class="title">ورود به سیستم</h1>
        <p class="subtitle">سامانه مدیریت مطالبات بانکی</p>
      </div>

      <form @submit.prevent="handleLogin" class="login-form">
        <!-- Username/ID field -->
        <div class="form-group">
          <label for="username" class="form-label">نام کاربری یا کد ملی</label>
          <div class="input-wrapper">
            <input
              id="username"
              v-model="form.username"
              type="text"
              placeholder="نام کاربری خود را وارد کنید"
              class="form-input"
              required
            />
            <span class="input-icon">👤</span>
          </div>
        </div>

        <!-- Password field -->
        <div class="form-group">
          <label for="password" class="form-label">رمز عبور</label>
          <div class="input-wrapper">
            <input
              id="password"
              v-model="form.password"
              :type="showPassword ? 'text' : 'password'"
              placeholder="رمز عبور خود را وارد کنید"
              class="form-input"
              required
            />
            <button
              type="button"
              @click="showPassword = !showPassword"
              class="toggle-password"
            >
              {{ showPassword ? '🙈' : '👁️' }}
            </button>
          </div>
        </div>

        <!-- Remember me checkbox -->
        <div class="remember-me">
          <input
            id="remember"
            v-model="form.remember"
            type="checkbox"
            class="checkbox"
          />
          <label for="remember" class="checkbox-label">یادم بمان</label>
        </div>

        <!-- Login button -->
        <button type="submit" class="login-btn" :disabled="isLoading">
          <span v-if="!isLoading">ورود به سیستم</span>
          <span v-else class="loading-spinner">⏳</span>
        </button>
      </form>

      <!-- Forgot password link -->
      <div class="footer-links">
        <a href="#" class="link">فراموشی رمز عبور؟</a>
      </div>

      <!-- Error message -->
      <div v-if="errorMessage" class="error-message">
        {{ errorMessage }}
      </div>

      <!-- Success message -->
      <div v-if="successMessage" class="success-message">
        {{ successMessage }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginPage',
  data() {
    return {
      form: {
        username: '',
        password: '',
        remember: false,
      },
      showPassword: false,
      isLoading: false,
      errorMessage: '',
      successMessage: '',
    };
  },
  methods: {
    async handleLogin() {
      this.errorMessage = '';
      this.successMessage = '';
      
      if (!this.form.username || !this.form.password) {
        this.errorMessage = 'لطفاً تمام فیلدها را پر کنید';
        return;
      }

      this.isLoading = true;
      
      try {
        // Simulate API call
        await new Promise((resolve) => setTimeout(resolve, 1500));
        
        this.successMessage = 'ورود موفق! در حال انتقال...';
        console.log('Login data:', {
          username: this.form.username,
          remember: this.form.remember,
        });
        
        // Reset form
        this.form = {
          username: '',
          password: '',
          remember: false,
        };
        
        // Redirect after 2 seconds
        setTimeout(() => {
          // this.$router.push('/dashboard'); // uncomment when ready
        }, 2000);
      } catch (error) {
        this.errorMessage = 'خطا در ورود. لطفاً دوباره تلاش کنید';
        console.error('Login error:', error);
      } finally {
        this.isLoading = false;
      }
    },
  },
};
</script>

<style scoped lang="css">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.login-container {
  position: relative;
  width: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  direction: rtl;
}

/* Animated background */
.animated-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    135deg,
    #667eea 0%,
    #764ba2 25%,
    #f093fb 50%,
    #4facfe 75%,
    #00f2fe 100%
  );
  background-size: 400% 400%;
  animation: gradientShift 15s ease infinite;
  z-index: 1;
}

@keyframes gradientShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

/* Glassmorphism card */
.login-card {
  position: relative;
  z-index: 2;
  width: 100%;
  max-width: 420px;
  padding: 40px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(31, 38, 135, 0.37);
  animation: slideUp 0.6s ease;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.card-header {
  text-align: center;
  margin-bottom: 30px;
}

.title {
  font-size: 28px;
  font-weight: 700;
  color: #fff;
  margin-bottom: 8px;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.subtitle {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
  font-weight: 400;
}

/* Form styles */
.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-label {
  font-size: 14px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  text-align: right;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.form-input {
  width: 100%;
  padding: 12px 15px 12px 45px;
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 12px;
  font-size: 14px;
  color: #fff;
  transition: all 0.3s ease;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.form-input::placeholder {
  color: rgba(255, 255, 255, 0.6);
}

.form-input:focus {
  outline: none;
  background: rgba(255, 255, 255, 0.25);
  border-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.2);
}

.input-icon {
  position: absolute;
  left: 15px;
  font-size: 18px;
  pointer-events: none;
}

.toggle-password {
  position: absolute;
  left: 15px;
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: rgba(255, 255, 255, 0.7);
  transition: color 0.3s ease;
  padding: 5px;
}

.toggle-password:hover {
  color: rgba(255, 255, 255, 1);
}

/* Checkbox styles */
.remember-me {
  display: flex;
  align-items: center;
  gap: 8px;
  justify-content: flex-end;
}

.checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #667eea;
}

.checkbox-label {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
  cursor: pointer;
  user-select: none;
}

/* Login button */
.login-btn {
  padding: 12px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border: none;
  border-radius: 12px;
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
  margin-top: 10px;
}

.login-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
}

.login-btn:active:not(:disabled) {
  transform: translateY(0);
}

.login-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.loading-spinner {
  display: inline-block;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Footer links */
.footer-links {
  text-align: center;
  margin-top: 20px;
}

.link {
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  font-size: 13px;
  transition: color 0.3s ease;
}

.link:hover {
  color: rgba(255, 255, 255, 1);
  text-decoration: underline;
}

/* Messages */
.error-message {
  padding: 12px;
  background: rgba(239, 68, 68, 0.2);
  border: 1px solid rgba(239, 68, 68, 0.5);
  border-radius: 8px;
  color: #fca5a5;
  font-size: 13px;
  text-align: center;
  animation: slideUp 0.3s ease;
}

.success-message {
  padding: 12px;
  background: rgba(34, 197, 94, 0.2);
  border: 1px solid rgba(34, 197, 94, 0.5);
  border-radius: 8px;
  color: #86efac;
  font-size: 13px;
  text-align: center;
  animation: slideUp 0.3s ease;
}

/* Responsive design */
@media (max-width: 480px) {
  .login-card {
    margin: 20px;
    padding: 30px 20px;
  }

  .title {
    font-size: 24px;
  }

  .form-input {
    padding: 11px 12px 11px 40px;
    font-size: 13px;
  }
}
</style>
