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
