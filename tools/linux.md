# 🐧 Linux Interview Questions & Answers

1. **A process is consuming high CPU — how do you investigate?**  
- Use top/htop, pidstat, strace, check logs.

2. **A server is running out of disk space — what’s your approach to clean it up?**  
- Use df, du, clean caches, remove logs and unused files.

3. **How do you find which process is using a specific port?**  
- Use lsof -i :port, netstat, or ss.

4. **How do you identify a process locking a file?**  
- Use lsof filename or fuser filename.

5. **How do you debug a service that fails to start?**  
- Use systemctl status, journalctl, check configs, run manually.

6. **How do you schedule recurring tasks with cron securely?**  
- Use crontab, secure scripts, restrict access, avoid sensitive data.

7. **How do you limit memory or CPU for a specific process?**  
- Use cgroups, systemd resource limits, nice, or cpulimit.

8. **How do you find zombie processes and clean them?**  
- Identify with ps, kill or restart parent process.

9. **How do you investigate high load average with normal CPU usage?**  
- Check I/O, processes in uninterruptible sleep, network and memory.

10. **How do you handle SSH key rotation securely on production servers?**  
- Generate/distribute keys securely, update authorized_keys atomically, remove old keys, audit logs.
