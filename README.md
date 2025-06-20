# Smart resume builder 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Smart Resume Builder</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6 font-sans">
  <div class="max-w-2xl mx-auto bg-white shadow-md p-6 rounded-lg">
    <h1 class="text-2xl font-bold mb-4 text-center">Smart Resume Builder with AI</h1>
    
    <div class="grid gap-4">
      <input type="text" id="name" placeholder="Full Name" class="border p-2 rounded w-full"/>
      <input type="email" id="email" placeholder="Email" class="border p-2 rounded w-full"/>
      <textarea id="summary" placeholder="Professional Summary" class="border p-2 rounded w-full"></textarea>
      <input type="text" id="skills" placeholder="Skills (comma separated)" class="border p-2 rounded w-full"/>
      <textarea id="education" placeholder="Education" class="border p-2 rounded w-full"></textarea>
      <textarea id="experience" placeholder="Experience" class="border p-2 rounded w-full"></textarea>
    </div>

    <button onclick="submitForm()" class="mt-4 bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">Submit</button>

    <div class="mt-6">
      <h2 class="font-semibold text-lg">AI Suggestions:</h2>
      <div id="suggestions" class="mt-2 bg-gray-100 p-3 rounded text-gray-800 whitespace-pre-wrap"></div>
    </div>
  </div>

  <script>
    async function submitForm() {
      const resume = {
        name: document.getElementById("name").value,
        email: document.getElementById("email").value,
        summary: document.getElementById("summary").value,
        skills: document.getElementById("skills").value.split(",").map(s => s.trim()),
        education: document.getElementById("education").value,
        experience: document.getElementById("experience").value
      };

      const prompt = `Suggest improvements for the following resume:\n\n${JSON.stringify(resume, null, 2)}`;

      try {
        const response = await fetch("https://api.openai.com/v1/chat/completions", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer YOUR_OPENAI_API_KEY" // Replace with your OpenAI key
          },
          body: JSON.stringify({
            model: "gpt-3.5-turbo",
            messages: [{ role: "user", content: prompt }]
          })
        });

        const data = await response.json();
        const suggestionText = data.choices?.[0]?.message?.content || "No suggestions found.";
        document.getElementById("suggestions").innerText = suggestionText;

      } catch (error) {
        document.getElementById("suggestions").innerText = "Error: " + error.message;
      }
    }
  </script>
</body>
</html>
![Screenshot_2025-06-20-19-40-15-19_e3c1f266f17b29c7b40472751f031275](https://github.com/user-attachments/assets/0facc2bc-a1a2-4d60-b72d-54ee46f8c019)

