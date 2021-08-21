FROM ubuntu:18.04

ENV IMAGE_NAME MikanOS
ENV APP_ROOT /usr/chapters

WORKDIR $APP_ROOT

RUN apt-get update
RUN apt-get install -y  vim \
                        build-essential \
                        python3 \
                        git \
                        okteta
                        # tmux \
                        # dosfstools

# https://github.com/uchan-nos/mikanos-build
# RUN cd $HOME; \
#     git clone https://github.com/uchan-nos/mikanos-build.git osbook; \
#     apt install -y ansible; \
#     cd $HOME/osbook/devenv; \
#     ansible-playbook -K -i ansible_inventory ansible_provision.yml

CMD ["/bin/bash"]

# Reference
# https://qiita.com/meruneru/items/0b7aa23ea505a80d60f5
