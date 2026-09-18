
> [!Tip] Como fazer uma lista do output
> Para pegar apenas os IPs e colocaramos todos na mesma linha separados por ",", usamos o comando:
> 
> ```bash
> cat smb.txt | grep "Up" | awk '{print $2}' | paste -sd, -
> ```
> 
