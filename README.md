# Ocnoclog
import { Suspense } from "react"
import ImageUploader from "@/components/image-uploader"
import ChatInterface from "@/components/chat-interface"
import { FlipHorizontalIcon as PipelineHorizontal } from "lucide-react"

export default function Home() {
  return (
    <main className="min-h-screen bg-gradient-to-b from-blue-50 to-white">
      <div className="container mx-auto px-4 py-8">
        <header className="mb-8">
          <div className="flex items-center gap-2">
            <PipelineHorizontal className="h-8 w-8 text-blue-600" />
            <h1 className="text-3xl font-bold text-blue-800">PipeFix AI</h1>
          </div>
          <p className="mt-2 text-gray-600">Upload images of pipe problems and get AI-powered solutions</p>
        </header>
        <div className="grid grid-cols-1 gap-8 lg:grid-cols-2">
          <div className="rounded-xl border bg-white p-6 shadow-sm">
            <h2 className="mb-4 text-xl font-semibold text-gray-800">Upload Pipe Image</h2>
            <Suspense fallback={<div>Loading image uploader...</div>}>
              <ImageUploader />
            </Suspense>
          </div>
<div className="rounded-xl border bg-white p-6 shadow-sm">
            <h2 className="mb-4 text-xl font-semibold text-gray-800">AI Diagnosis & Solutions</h2>
            <Suspense fallback={<div>Loading chat interface...</div>}>
              <ChatInterface />
            </Suspense>
          </div>
        </div>

<div className="mt-8 rounded-xl border bg-white p-6 shadow-sm">
          <h2 className="mb-4 text-xl font-semibold text-gray-800">How It Works</h2>
          <div className="grid grid-cols-1 gap-6 md:grid-cols-3">
            <div className="rounded-lg bg-blue-50 p-4">
              <div className="mb-2 flex h-10 w-10 items-center justify-center rounded-full bg-blue-600 text-white">
                1
              </div>
              <h3 className="mb-2 font-medium">Upload an Image</h3>
              <p className="text-sm text-gray-600">
                Take a clear photo of the pipe problem and upload it to our system.
              </p>
            </div>
            <div className="rounded-lg bg-blue-50 p-4">
              <div className="mb-2 flex h-10 w-10 items-center justify-center rounded-full bg-blue-600 text-white">
                2
              </div>
              <h3 className="mb-2 font-medium">AI Analysis</h3>
              <p className="text-sm text-gray-600">Our AI will analyze the image to identify the pipe issue.</p>
            </div>
            <div className="rounded-lg bg-blue-50 p-4">
              <div className="mb-2 flex h-10 w-10 items-center justify-center rounded-full bg-blue-600 text-white">
                3
              </div>
              <h3 className="mb-2 font-medium">Get Solutions</h3>
              <p className="text-sm text-gray-600">
                Receive detailed explanations and step-by-step solutions to fix the problem.
              </p>
            </div>
          </div>
        </div>
      </div>
    </main>
  )
}

import type React from "react"
import type { Metadata } from "next"
import { Inter } from "next/font/google"
import "./globals.css"
import { ThemeProvider } from "@/components/theme-provider"

const inter = Inter({ subsets: ["latin"] })

export const metadata: Metadata = {
  title: "PipeFix AI - Pipe Problem Detection & Solutions",
  description: "Upload images of pipe problems and get AI-powered solutions",
}

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode
}>) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <ThemeProvider attribute="class" defaultTheme="light" enableSystem disableTransitionOnChange>
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}
import { openai } from "@ai-sdk/openai"
import { streamText } from "ai"
import type { NextRequest } from "next/server"

export const runtime = "nodejs"

export async function POST(req: NextRequest) {
  try {
    const body = await req.json()
    const { messages } = body

   // Check if we have messages
    if (!messages || !Array.isArray(messages)) {
      return new Response(JSON.stringify({ error: "Messages are required" }), {
        status: 400,
        headers: { "Content-Type": "application/json" },
      })
    }

   // Stream the response from the AI
    const result = streamText({
      model: openai("gpt-4o"),
      messages,
      temperature: 0.7,
      maxTokens: 1000,
    })

   // Return the streamed response
    return result.toDataStreamResponse()
  } catch (error) {
    console.error("Error in chat API:", error)
    return new Response(JSON.stringify({ error: "Failed to process request" }), {
      status: 500,
      headers: { "Content-Type": "application/json" },
    })
  }
}

