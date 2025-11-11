# Poor Man’s claude code automation 

Do you have a job to be done? And you are out of claude limits and have to wait for 5 hours?? Don't worry, there's a poor man's simple solution!! :

# First, create Claude systemd service (/etc/systemd/system/claude.service):

[Unit] Description=Claude Background Job After=network.target

 [Service] Type=simple User=mert WorkingDirectory=/home/mert/Masaüstü/comment-pipeline-service/new-ui/ ExecStart=/usr/bin/claude --dangerously-skip-permissions "Hello, you are now automatically receiving this message. Your task is to convert all Radix UI components in the system to ShadCN components. Start immediately, analyze, and proceed without asking for approval. Continue until the entire system is finished. Use pnpm and run 'pnpm run build' to check for compilation errors." Restart=on-failure

Step two, create a wake up the suspended computer and run claude service timer:

# Timer with wakeup (/etc/systemd/system/claude.timer)

[Unit] Description=Run Claude at scheduled time and wake PC from suspend

[Timer] OnCalendar=2025-08-19 08:03:00 Persistent=true WakeSystem=true Unit=claude.service

# [Install] WantedBy=timers.target

Step three, execute this lines:

sudo systemctl daemon-reload sudo systemctl enable --now claude.timer

Last step: when you wake up check the logs:

systemctl list-timers --all journalctl -u claude.service -f

Upvote8Downvote6Go to comments