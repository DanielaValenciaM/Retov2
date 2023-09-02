<template>
  <div></div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f8f8;
  border: 1px solid #e0e0e0;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.header {
  font-size: 28px;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}

.upload-section {
  text-align: center;
  margin-bottom: 20px;
}

.file-label {
  display: block;
  font-size: 18px;
  color: #333;
  cursor: pointer;
}

.file-input {
  display: none;
}

.data-section {
  border: 1px solid #ccc;
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
  margin-top: 20px;
}

.data-header {
  font-size: 24px;
  color: #333;
  margin-bottom: 10px;
  text-align: center;
}

.data-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.data-item {
  width: 30%;
  margin: 10px;
  padding: 10px;
  border: 1px solid #eee;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.data-name {
  font-weight: bold;
  color: #555;
}

.data-email,
.data-phone {
  color: #777;
}

.download-button {
  display: block;
  margin: 20px auto;
  background-color: #007bff;
  color: #fff;
  font-size: 18px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.download-button:hover {
  background-color: #0056b3;
}
</style>
