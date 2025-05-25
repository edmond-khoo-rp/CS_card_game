// DataDefenseGame.jsx
import React, { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { motion } from "framer-motion";

const questions = [
  {
    question: "You receive an email from an unknown sender asking for your login credentials. What should you do?",
    options: ["Click and enter credentials", "Report as phishing", "Ignore and delete"],
    correct: 1
  },
  {
    question: "What is the best way to create a strong password?",
    options: ["Use your birthdate", "Use '123456'", "Use a mix of letters, numbers, and symbols"],
    correct: 2
  },
  {
    question: "Which tool helps hide your internet activity from others?",
    options: ["VPN", "Bluetooth", "Cookies"],
    correct: 0
  },
  {
    question: "What should you do if you suspect your account has been hacked?",
    options: ["Ignore it", "Change password and report it", "Tell a friend"],
    correct: 1
  },
  {
    question: "Which of the following is a secure way to share sensitive files?",
    options: ["Email without encryption", "Over a phone call", "Using an encrypted cloud service"],
    correct: 2
  },
  {
    question: "Which is considered personal identifiable information (PII)?",
    options: ["IP address", "Favorite color", "Age range"],
    correct: 0
  },
  {
    question: "What does two-factor authentication (2FA) add to your account?",
    options: ["More ads", "Stronger security", "Faster login"],
    correct: 1
  },
  {
    question: "How often should you update your passwords?",
    options: ["Never", "Every few months", "Only when prompted"],
    correct: 1
  },
  {
    question: "What should you avoid when using public Wi-Fi?",
    options: ["Browsing memes", "Online banking", "Watching videos"],
    correct: 1
  },
  {
    question: "Which of these is a sign of a phishing website?",
    options: ["HTTPS", "Spelling mistakes and fake logos", "Padlock icon"],
    correct: 1
  }
];

const leaderboardKey = "dataPrivacyLeaderboard";

function getLeaderboard() {
  const saved = localStorage.getItem(leaderboardKey);
  return saved ? JSON.parse(saved) : [];
}

function updateLeaderboard(score) {
  const name = "Guest";
  const leaderboard = getLeaderboard();
  leaderboard.push({ name, score });
  leaderboard.sort((a, b) => b.score - a.score);
  localStorage.setItem(leaderboardKey, JSON.stringify(leaderboard.slice(0, 5)));
}

export default function DataDefenseGame() {
  const [current, setCurrent] = useState(0);
  const [score, setScore] = useState(0);
  const [selected, setSelected] = useState(null);
  const [showAnswer, setShowAnswer] = useState(false);
  const [timer, setTimer] = useState(10);
  const [gameOver, setGameOver] = useState(false);
  const [leaderboard, setLeaderboard] = useState(getLeaderboard());

  useEffect(() => {
    if (timer > 0 && !showAnswer && !gameOver) {
      const countdown = setTimeout(() => setTimer(timer - 1), 1000);
      return () => clearTimeout(countdown);
    } else if (timer === 0 && !showAnswer && !gameOver) {
      setShowAnswer(true);
    }
  }, [timer, showAnswer, gameOver]);

  const handleOptionClick = (index) => {
    if (showAnswer) return;
    setSelected(index);
    setShowAnswer(true);
    if (index === questions[current].correct) {
      setScore(score + 1);
    }
  };

  const nextQuestion = () => {
    setSelected(null);
    setShowAnswer(false);
    setTimer(10);
    if (current + 1 < questions.length) {
      setCurrent(current + 1);
    } else {
      setGameOver(true);
      updateLeaderboard(score);
      setLeaderboard(getLeaderboard());
    }
  };

  return (
    <div className="p-8 flex flex-col items-center justify-center min-h-screen bg-gray-100">
      <motion.div
        className="max-w-md w-full"
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.3 }}
      >
        <Card className="rounded-2xl shadow-lg bg-white">
          <CardContent className="p-6 text-center">
            {!gameOver ? (
              <>
                <h2 className="text-xl font-bold mb-2">Question {current + 1}</h2>
                <p className="text-base mb-4">⏳ Time left: {timer}s</p>
                <p className="text-lg font-semibold mb-4">{questions[current].question}</p>
                {questions[current].options.map((option, index) => (
                  <button
                    key={index}
                    onClick={() => handleOptionClick(index)}
                    className={`block w-full text-left p-3 mb-2 rounded-lg border 
                      ${
                        showAnswer
                          ? index === questions[current].correct
                            ? 'bg-green-200 border-green-600'
                            : index === selected
                            ? 'bg-red-200 border-red-600'
                            : 'bg-white'
                          : 'hover:bg-blue-100 border-gray-300'
                      }`}
                  >
                    {option}
                  </button>
                ))}
                {showAnswer && (
                  <Button onClick={nextQuestion} className="mt-4">Next</Button>
                )}
              </>
            ) : (
              <div>
                <h2 className="text-2xl font-bold mb-2">🎉 Game Over!</h2>
                <p className="text-lg mb-4">Your Score: {score} / {questions.length}</p>
                <h3 className="text-xl font-semibold mb-2">🏆 Leaderboard</h3>
                <ul className="text-left">
                  {leaderboard.map((entry, idx) => (
                    <li key={idx} className="mb-1">{idx + 1}. {entry.name} – {entry.score}</li>
                  ))}
                </ul>
              </div>
            )}
          </CardContent>
        </Card>
      </motion.div>
    </div>
  );
}
