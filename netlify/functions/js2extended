const { stream } = require("@netlify/functions");

exports.handler = stream(async (event) => ({
  // Get the request from the request query string, or use a default
  pie:
    event.queryStringParameters?.pie ??
    "something inspired by a springtime garden",

  res: await fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      // Set this environment variable to your own key
      Authorization: `Bearer ${process.env.OPENAI_API_KEY}`,
    },
    body: JSON.stringify({
      model: "gpt-3.5-turbo",
      messages: [
        {
          role: "system",
          content:
            "You are a baker. The user will ask you for a pie recipe. You will respond with the recipe. Use markdown to format your response",
        },
        // Using "slice" to limit the length of the input to 500 characters
        { role: "user", content: pie.slice(0, 500) },
      ],
      // Use server-sent events to stream the response
      stream: true,
    }),
  }),
  headers: {
    // This is the mimetype for server-sent events
    "content-type": "text/event-stream",
  },
  statusCode: 200,
  // Pipe the event stream from OpenAI to the client
  body: res.body,
}));
