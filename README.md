README: Ultimate AI Agent in n8n
Overview
The Ultimate AI Agent is a powerful, multi-functional AI assistant integrated into n8n. It intelligently routes user queries to the appropriate tool and ensures that tasks are executed efficiently. The system consists of multiple agents and workflows, enabling seamless automation of various tasks, including:

Email management
Calendar event scheduling
Contact retrieval & updates
Mathematical calculations
Weather forecasts
Web searches (Tavily-based)
Google Maps data retrieval
Content generation
This README provides details on how the Ultimate AI Agent operates, how it's structured, and how you can expand it.

System Architecture
The Ultimate AI Agent is composed of multiple specialized agents, each designed for a specific function. The main agent determines which sub-agent to call based on the user's input.

Agents & Their Functions
Agent Name	Function
Ultimate AI Agent	The core router that determines which agent to use based on the query.
Email Agent	Sends, drafts, or manages emails.
Calendar Agent	Creates, updates, and deletes calendar events.
Contact Agent	Retrieves, updates, and adds contacts.
Tavily Place Agent	Searches for location-based results (restaurants, landmarks, venues, etc.) and formats them with Google Maps links.
Tavily General Agent	Handles general web searches for topics like trending news, book recommendations, and top music charts.
Weather Agent	Retrieves current weather conditions and forecasts.
Calculator Agent	Performs numerical calculations and conversions.
Memory Agent	Stores and retrieves memory using Pinecone Vector Database for long-term knowledge retention.
Query Processing Workflow
User Input: The user submits a request.
Ultimate AI Agent Processes the Query:
Determines intent (email, search, calculation, etc.).
Routes the query to the correct agent.
Sub-Agent Execution: The corresponding agent processes the request.
Formatted Response: The AI ensures output formatting consistency.
Final Delivery: The response is sent back to the user (e.g., via Telegram).
Search Handling: Tavily Place vs. Tavily General
The Ultimate AI Agent determines whether a search is place-related or general:

✅ Tavily Place Agent (Uses Google Maps API)

Extracts place-related queries (e.g., restaurants, landmarks, venues).
Formats results with Google Maps links, ratings, price range, and descriptions.
If missing, dynamically generates Google Maps URLs.
✅ Tavily General Agent (Standard Web Search)

Handles non-place searches (e.g., "top-selling books 2024", "latest tech trends").
Returns general information without additional formatting.
Example Queries & Routing
User Query	Agent Used	Response Formatting
"Best pizza restaurants in Chicago"	Tavily Place Agent	Google Maps links + formatted results
"Trending movies of 2024"	Tavily General Agent	Standard AI-generated response
"How do black holes form?"	No Tavily (AI answers directly)	Direct response
Example System Prompt for Ultimate AI Agent
The Ultimate AI Agent follows a structured system prompt to determine query intent:

plaintext
Copy
Edit
Role: AI-Powered Task Router  
- Route all queries to the correct agent.  
- NEVER execute tasks yourself.  
- ENSURE all required information is present before execution.  
- ASK for clarification if key details are missing.

### Query Routing Rules:
- **Email Requests:** Use `Contact Agent` first → Send request to `Email Agent`.  
- **Calendar Events:** Ensure **title, date/time, duration, attendees** → Use `Calendar Agent`.  
- **Calculations:** Send all equations to `Calculator Agent`.  
- **Weather Forecasts:** Extract location → Use `Weather Agent`.  
- **Memory Retrieval:** Use short-term memory first, then Pinecone (long-term).  

### Web Searches (Tavily Rules)
- **If search is place-based:** Use `Tavily Place Agent` (Google Maps API formatting).  
- **If search is general:** Use `Tavily General Agent`.  
- **If search can be answered internally:** Answer without Tavily.

### Example Routing Logic:
{ "search_type": "place", "query": "Best sushi in NYC" }
{ "search_type": "general", "query": "Top 10 Netflix shows in 2024" }
{ "use_tavily": false, "query": "Who was Nikola Tesla?" }

### Output Formatting for Places:
- Name (Google Maps hyperlink)
- Rating
- Price Range ($X-$Y)
- Description
- Location

Ensure all formatted results are clear and user-friendly.
Google Maps Integration
The Tavily Place Agent ensures that every place result includes a Google Maps link. If a direct link is missing, it is dynamically generated.

Google Maps Link Generation (JavaScript Code)
javascript
Copy
Edit
const results = $json.results || [];

return results.map(place => ({
    name: `[${place.title}](https://maps.google.com/?q=${encodeURIComponent(place.title)})`,
    rating: place.score ? `${place.score} stars` : "Rating not available",
    price: place.price ? `$${place.price}` : "Price not available",
    description: place.content || "No description provided",
    location: place.raw_content || "Location not available"
}));
Memory Handling
The agent stores and retrieves user preferences, past interactions, and important context using:

Short-term memory (Window Buffer Memory)
Long-term memory (Pinecone Vector Database)
💡 Future Expansion: Consider adding a mechanism for memory expiration and auto-updates.

Installation & Setup
Prerequisites
Self-Hosted n8n or Cloud Version
API Keys Required:
Tavily API (Web Search)
Google Maps API (Location Data)
Pinecone API (Long-Term Memory)
Email Provider API (Gmail, Outlook, etc.)
Steps to Install
Clone or Import Workflow into n8n
Import the provided JSON workflow into your n8n instance.
Set Up API Credentials
Navigate to Settings → API Credentials and add required API keys.
Test Each Agent Individually
Run Tavily Place Agent and Tavily General Agent separately to confirm they return structured results.
Connect Ultimate AI Agent to Telegram
Ensure the final output is routed to Telegram (or another messaging platform).
Future Enhancements
✅ Improve Context Awareness – Enhance AI’s ability to decide when to use Tavily vs. internal knowledge.
✅ Expand Memory Capabilities – Store more structured data for better long-term personalization.
✅ Integrate Additional APIs – Pull real-time price estimates from Google Places, OpenTable, or Yelp.
✅ Voice Assistant Compatibility – Extend functionality to voice-based assistants like Siri or Google Assistant.

Conclusion
The Ultimate AI Agent is designed to streamline AI-powered automation in n8n, ensuring accurate routing, formatted search results, and seamless integrations. With expandable workflows and enhanced search classification, this AI assistant provides structured, reliable, and intelligent responses across multiple domains.

🚀 Start using the Ultimate AI Agent today! 🚀
