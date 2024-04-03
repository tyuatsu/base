Utilizando o "Read the Docs" by Tyuatsu
===============================
Escrever é uma tarefa difícil. Isso torna a documentação uma arte. Para isto, algumas soluções a auxiliam. Uma delas é o https://readthedocs.org/.

* GitHub, GiLab ou Bitbucket
* Read the Docs
* Sphinx


Linux
-----------
Para este procedimento foi utilizado o ``FEDORA 29 Workstation``.::

        sudo dnf -y install git python-sphinx
        git clone https://github.com/conta/repositorio
        cd repositorio
        git init
        sphinx-quickstart
        make html
        git add --all
        git commit -a -m "A Arte Ninja de Documentar"
        git push
